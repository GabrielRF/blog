---
layout: post
title: "Descrição automática de imagens no Telegram usando ChatGPT-4o"
date: 2024-05-25 11:00:00 -0300
categories: [Telegram, Automação]
tags: telegram automação openai descrição acessibilidade
image: "/assets/img/ChatGPT4o.png"
---

Descrição das duas imagens do post:

> A imagem mostra uma estrutura arquitetônica icônica, possivelmente a Catedral Metropolitana de Brasília, projetada por Oscar Niemeyer. A catedral possui um design distinto, com 16 colunas de concreto que se curvam em direção ao céu, criando uma forma circular e coroada por uma cruz metálica. As colunas têm uma aparência que lembra mãos interligadas em oração, simbolizando encontro e elevação espiritual. Na frente da estrutura, algumas pessoas estão reunidas na área externa. O céu está parcialmente nublado, com tons alaranjados indicativos do entardecer, conferindo uma iluminação suave e dinâmica à cena.

> A imagem é um banner promocional para um evento intitulado "Concurso Nacional Unificado". À esquerda do banner, em letras grandes e azuis, lê-se "Concurso Nacional Unificado". Na parte inferior esquerda, há os logos do Ministério da Gestão e da Inovação em Serviços Públicos e do Governo Federal, que inclui a bandeira do Brasil estilizada acompanhada do texto "União e Reconstrução". À direita da imagem, há a foto em preto e branco de uma mulher jovem, sorrindo, segurando um caderno de prancheta. A foto dela está emoldurada por um contorno verde e superposta a várias formas geométricas coloridas, como um círculo vermelho com listras brancas e um triângulo amarelo com um círculo azul dentro. O fundo do banner é predominantemente branco, com detalhes coloridos nos cantos superiores esquerdo e inferior direito, seguindo um estilo moderno e vibrante.

# ChatGPT-4o

No dia 13 de Maio de 2024 a OpenAI lançou a versão mais nova do ChatGPT, a versão 4o. Segundo a própria OpenAI:

> GPT-4o (“o” de “omni”) é um passo em direção a uma interação humano-computador muito mais natural—ele aceita como entrada qualquer combinação de texto, áudio, imagem e vídeo e gera qualquer combinação de saídas de texto, áudio e imagem. Ele pode responder a entradas de áudio em apenas 232 milissegundos, com uma média de 320 milissegundos, o que é semelhante ao tempo de resposta humano em uma conversa. Ele iguala o desempenho do GPT-4 Turbo em texto em inglês e código, com melhorias significativas em texto em línguas não inglesas, além de ser muito mais rápido e 50% mais barato na API. O GPT-4o é especialmente melhor na compreensão de visão e áudio em comparação com os modelos existentes. [^gpt4o-texto]

Os vídeos do lançamento são impressionantes[^gpt4o-playlist]. Um vídeo em particular chamou minha atenção:

<iframe width="560" height="315" src="https://www.youtube.com/embed/KwNUJ69RbwY?si=aU4TdTWlCJmbeeaF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Motivação

Participo de um grupo no Telegram que possui, dentre suas recomendações e regras, a orientação de que descrevam todas as imagens. A meta é que todas as pessoas sejam capazes de participar das conversas, independente de qualquer coisa. Ao assistir o vídeo acima, pensei: _Por que não automatizar a descrição de imagens?_

# Bot

A integração com um bot do Telegram foi relativamente simples. Segui o guia oficial[^guia] e funcionou de maneira excepcional. As descrições, em sua maioria, ficam excelentes!

> Como a funcionalidade é paga, o bot não é público. Caso crie o seu, não se esqueça de consultar os preços no site. [^precos]

## requirements.txt
Crie o arquivo `requirements.txt` com o conteúdo:

```text
pyTelegramBotAPI==4.15.4
requests==2.31.0
```

Instale as dependências usando o comando:
```shell
pip install -r requirements.txt
```

## bot.py
```python
import base64
import requests
import telebot

# Valores necessários ao funcionamento
BOT_TOKEN = <Token do bot>
OPENAI_KEY = <Api key da OpenAI>

# Cria bot
bot = telebot.TeleBot(BOT_TOKEN)

# Baixa a imagem e retorna o nome do arquivo
def download_photo(message):
    photo_info = bot.get_file(message.photo[-1].file_id)
    photo_download = bot.download_file(photo_info.file_path)
    photo_name = f'{photo_info.file_unique_id[:6]}.{photo_info.file_path.split(".")[-1]}'
    with open(photo_name, 'wb') as new_photo:
        new_photo.write(photo_download)
    return photo_name

# Faz o encode da imagem para base64
def encode_image(photo_name):
  with open(photo_name, "rb") as photo_file:
    return base64.b64encode(photo_file.read()).decode('utf-8')

# Gera a descrição da foto
def describe_photo(photo_encoded):
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {OPENAI_KEY}"
    }
    payload = {
        "model": "gpt-4o",
        "messages": [
            {
                "role": "user",
                "content": [
                    {
                        "type": "text",
                        "text": "Descreva a imagem"
                    },
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": f"data:image/jpeg;base64,{photo_encoded}"
                        }
                    }
                ]
            }
        ],
        "max_tokens": 500
    }
    response = requests.post("https://api.openai.com/v1/chat/completions", headers=headers, json=payload)
    return response.json()['choices'][0]['message']['content']

# Remove a foto do disco de armazenamento
def remove_photo(photo_name):
    os.remove(photo_name)

# Recebimento de imagens e envio de descrição
@bot.message_handler(content_types=['photo'])
def get_photo(message):
    photo_name = download_photo(message)
    photo_encoded = encode_image(photo_name)
    description = describe_photo(photo_encoded)
    text = (
        f'🖼  <b>Descrição automática:</b>'
        f'\n<blockquote>{description}</blockquote>'
    )
    bot.send_message(
        message.chat.id,
        text,
        parse_mode='HTML',
        reply_to_message_id=message.id
    )
    remove_photo(photo_name)

bot.infinity_polling()
```

Sendo:
* `BOT_TOKEN`: O token do seu bot, obtido no [@BotFather](https://t.me/BotFather);
* `OPENAI_KEY`: Chave de API da OpenAI, obtida em [https://platform.openai.com/api-keys](https://platform.openai.com/api-keys).

O bot é executado pelo comando[^systemctl]:
```shell
python bot.py
```

--- 

[^gpt4o-texto]: Texto de apresentação: [https://openai.com/index/hello-gpt-4o/](https://openai.com/index/hello-gpt-4o/)

[^gpt4o-playlist]: Playlist de lançamento do ChatGPT 4o: [https://www.youtube.com/playlist?list=PLOXw6I10VTv8VOvPNVQ8c4D4NyMRMotXh](https://www.youtube.com/playlist?list=PLOXw6I10VTv8VOvPNVQ8c4D4NyMRMotXh)

[^guia]: Guia oficial de implementação do Vision: [https://platform.openai.com/docs/guides/vision](https://platform.openai.com/docs/guides/vision)

[^precos]: Preços do ChatGPT 4o: [https://openai.com/api/pricing/](https://openai.com/api/pricing/)

[^systemctl]: Veja o post "[Como manter um processo rodando para sempre](/posts/Systemctl/)"
