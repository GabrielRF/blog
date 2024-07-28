---
layout: post
title: "Notificando inscritos da Twitch via Telegram"
date: 2024-07-28 20:00:00 -0300
categories: [Telegram, Automação]
tags: telegram twitch notificacoes inscritos canal
image: "/assets/img/Twitch2Telegram.png"
---

# Twitch

Twitch é um site em que milhões de pessoas se reúnem diariamente para conversar, interagir e criar seu próprio conteúdo. Tem como principal atrativo as transmissões ao vivo, chamadas de <i>lives</i>, em que o bate-papo, <i>chat</i>, interage entre si e com o criador do conteúdo. No entanto, assim como em outras plataformas, as notificações aos inscritos podem atrasar ou até mesmo nunca chegar, deixando parte da audiência de fora das transmissões.

# Telegram

Canais do Telegram são parecidos com grupos de conversas, com a diferença que somente os administradores podem falar. <b>Um canal entrega a notificação para todos os inscritos simultaneamente</b>, sem limite de quantidade, sendo ideal para grandes públicos[^canais]. Grupos, por exemplo, são limitados a 200.000 pessoas. Já os bots enviam apenas 30 mensagens por segundo[^rate-limit].

[^canais]: [Telegram não aumenta engajamento](/posts/TelegramInfluencer/#canais).

[^rate-limit]: [Broadcasting to Users](https://core.telegram.org/bots/faq#broadcasting-to-users).

# Integrando as notificações da Twitch ao Telegram

## Aplicação na Twitch

O primeiro passo é a criação de uma aplicação na Twitch. Ela é importante para a obtenção dos dados da transmissão.

1. Acesse [dev.twitch.tv/console](https://dev.twitch.tv/console);
2. Crie uma aplicação nova em <b>Register Your Application</b>;
3. Preencha os campos:
* `Name`: Nome de sua aplicação.
* `OAuth Redirect URLs`: Preencha com `http://localhost`;
* `Category`: `Confidential`.
4. Salve os valores de `Client ID` e `Client Secret`.

## Bot no Telegram

A função do bot é o envio da mensagem do canal. Seu nome ou <i>@nome_de_usuário</i> são irrelevantes, pois não serão vistos pelos inscritos.

1. Fale com o [@BotFather](https://t.me/BotFather) e crie um bot[^bots];
2. Salve o valor do `Token`;
3. Caso o canal não exista, crie-o e salve seu ID[^IDs];
4. Adicione o bot como administrador do canal.

[^bots]: [Como fazer bots para o Telegram](/posts/HowToBot/)

## Script Python

### requirements.txt

Instale as bibliotecas abaixo:

```text
pytelegrambotapi==4.21.0
requests==2.28.1
```

### twitch2telegram

O script abaixo é responsável por verificar a existência de lives, gerar a <i>thumbnail</i> e fazer os envios.

```python
import datetime
import pytz
import requests
import telebot

## Twitch
client_id = "<Client ID da Twitch>"
client_secret = "<Client Secret da Twitch>"
streamer = "<Nome da Stream>"

## Telegram
token = "<Token do bot>"
destination = <ID de destino da mensagem>

## Ajustes
minutos = 5

## Verifica os dados da stream
def get_data():
    body = {
        'client_id': client_id,
        'client_secret': client_secret,
        'grant_type': 'client_credentials'
    }

    r = requests.post('https://id.twitch.tv/oauth2/token', body)
    keys = r.json();

    headers = {
        'Client-ID': client_id,
        'Authorization': 'Bearer ' + keys['access_token']
    }

    stream = requests.get(
        'https://api.twitch.tv/helix/streams?user_login='
        + streamer,
        headers=headers
    )
    stream_data = stream.json();

    try:
        return stream_data['data'][0]
    except IndexError:
        return None

## Gera a thumbnail
def get_thumbnail(live_data):
    thumbnail_url = live_data['thumbnail_url']
    thumbnail_url = thumbnail_url.replace('{width}x{height}', '1280x720')
    response = requests.get(thumbnail_url)
    open('thumb.png', 'wb').write(response.content)
    return open('thumb.png', 'rb')

## Envia mensagem
def send_message(thumbnail, title):
    bot = telebot.TeleBot(token)
    message = (
        f'🔴 <b>Alerta de Live!</b>\n'
        + f'<blockquote>{title}</blockquote>'
    )
    btn_link = telebot.types.InlineKeyboardMarkup()
    btn = telebot.types.InlineKeyboardButton(f'📹 Twitch', url=f'https://www.twitch.tv/{streamer}')
    btn_link.row(btn)
    msg = bot.send_photo(
        destination,
        thumbnail,
        caption=message,
        parse_mode='HTML',
        reply_markup=btn_link
    )

## Verifica há quanto tempo a transmissão está funcionando
def check_time():
    start_time = datetime.datetime.strptime(
        live_data['started_at'],
        '%Y-%m-%dT%H:%M:%S%z'
    )
    time_diff = (
        datetime.datetime.utcnow().replace(tzinfo=pytz.utc)
        - start_time
    )
    if time_diff < datetime.timedelta(minutes=minutos):
        return True
    return False


if __name__ == "__main__":
    live_data = get_data()
    if live_data and check_time():
        title = live_data['title']
        thumbnail = get_thumbnail(live_data)
        send_message(thumbnail, title)
```

Para que o código funcione, ajuste as variáveis:

* `client_id`: Com o valor fornecido pela Twitch no passo [Aplicação na Twitch](#aplicação-na-twitch);
* `client_secret`: Com o valor fornecido pela Twitch no passo [Aplicação na Twitch](#aplicação-na-twitch); 
* `streamer`: Nome do streamer;
* `token`: Token do bot criado no passo [Bot no Telegram](#bot-no-telegram);
* `destination`: ID de destino da mensagem[^IDs];
* `minutos`: Intervalo de checagem. Exemplo: Se a checagem for feita a cada cinco minutos, o valor deve ser `5`.

> A variável `minutos` tem a função de simplificar o código. Caso a transmissão tenha mais tempo de duração que o valor desta variável, nenhuma mensagem é enviada. Caso contrário, é entendido que a live começou há pouco tempo, portanto, a mensagem é enviada no canal.

O script python pode ser rodado de inúmeras formas. Recomendo o uso de `cronjobs` ou até como um serviço do sistema operacional[^Systemctl].

[^IDs]: [Como obter IDs no Telegram](/posts/TelegramID/)
[^Systemctl]: [Como manter um processo rodando para sempre](/posts/Systemctl)

# Exemplos de canais

Dois exemplos de canais que usam o script deste post:

* [@NaoSigo](https://t.me/NaoSigo), com atualizações das lives e vídeos do Cid Cidoso (Não Salvo).
* [@BotaNoGau](https://t.me/BotaNoGau), com informações das streams do Gaules.

---
