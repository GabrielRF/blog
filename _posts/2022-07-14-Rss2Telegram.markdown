---
layout: post
title: "Feed RSS no Telegram sem precisar de servidor"
date: 2022-07-14 20:50:00 -0300
categories: [Telegram, Bot]
tags: telegram bot canal automação github rss
image: "/assets/img/rsslogo.png"
---

Envio automático de feed RSS no Telegram! Funciona com envios para pessoas, grupos e canais, públicos ou privados.

## Configuração

Faça um fork do [repositório original](https://github.com/GabrielRF/Rss2Telegram), clicando em `fork`.

### Obrigatórias

Vá em `Settings`, `Secrets`, `Actions` e crie a variável:

* `BOT_TOKEN`: Token do bot que fará os envios. Fale com o [@BotFather](https://t.me/BotFather) caso não tenha um;

* `DESTINATION`: IDs ou `@` de quem receberá as mensagens;

* `URL`: Lista de urls dos feeds separados por vírgulas.

> As variáveis `DESTINATION` e `URL` podem ser criadas em `Secrets` ou em arquivos `.txt`, prevalecendo o que estiver em `Secrets`. Ou seja, caso crie `DESTINATION.txt` ou `URL.txt` é necessário remover o respectivo `Secret`.

### Opcionais

* `MESSAGE_TEMPLATE`: Mensagem a ser enviada. Veja a lista de componentes abaixo;

* `BUTTON_TEXT`: Texto do botão a ser enviado. Veja a lista de componentes abaixo;

* `EMOJIS`: Emojis que serão sorteados aleatoriamente a cada envio. Lista pré-definida:🗞,📰,🗒,🗓,📋,🔗,📝,🗃;

* `PARAMETERS`: Parâmetros a serem acrescentados ao link. Exemplo: `utm_source=telegram&utm_medium=telegrammessage&utm_campaign=rss2telegram`.

### Componentes

Tanto o corpo da mensagem quanto o botão podem conter textos, emojis e/ou um dos componentes pré-definidos.

* `{SITE_NAME}` Contém o nome do site;

* `{TITLE}` É o título do post;

* `{SUMMARY}` Corpo do texto do tópico do RSS;

* `{LINK}` Link para o post completo;

* `{EMOJI}` Emojis que serão sorteados aleatoriamente no momento do envio. Crie sua lista de emojis na variável EMOJIS.

### Filtros

Por padrão, todos o conteúdo do feed é enviado. Para criar regras de filtros, crie um arquivo chamado `RULES.txt` e adicione as regras conforme a necessidade. As regras serão lidas em ordem!

> O valor de `termo` independe de minúsculas ou maiúsculas

`ACCEPT:ALL`: Todas as mensagens serão enviadas;

`DROP:ALL`: Todas as mensagens não serão enviadas;

`ACCEPT:termo`: A mensagem será enviada se `termo` estiver presente;

`DROP:termo`: A mensagem não será enviada se `termo` estiver presente.

### Ação

Ajustadas as variáveis, vá em `Actions`, `Select workflow` e clique em `Enable workflow`. A ação irá rodar a cada 1 hora para verificar as atualizações.

Para alterar o intervalo da ação basta alterar o arquivo `.github/workflows/cron.yml`.

Exemplos:

`- cron: '0 */1 * * *'`: Faz a ação ser executada uma vez por hora;

`- cron: '*/15 * * * *'`: Faz a ação ser executada uma vez a cada quinze minutos.

## Exemplos de mensagens

### Mensagem padrão

<center>
<img src="/assets/img/rss2telegram_default.jpg" alt="Mensagem padrão." style="width:50%">
<br><small>Mensagem padrão</small>
</center>

`MESSAGE_TEMPLATE`: Não definido;

`BUTTON_TEXT`: Não definido.

### Mensagem padrão com botão definido

<center>
<img src="/assets/img/rss2telegram_button.jpg" alt="Mensagem padrão com botão definido." style="width:50%">
<br><small>Mensagem padrão com botão definido</small>
</center>

`MESSAGE_TEMPLATE`: `{EMOJI}{TITLE}`;

`BUTTON_TEXT`: `{TITLE}`.

### Mensagem padrão com link no texto

<center>
<img src="/assets/img/rss2telegram_nobutton.jpg" alt="Mensagem padrão com link no texto." style="width:50%">
<br><small>Mensagem padrão com link no texto</small>
</center>

`MESSAGE_TEMPLATE`: `{EMOJI}<b>{TITLE}</b>\n\n{LINK}`;

`BUTTON_TEXT`: Não definido.

## Exemplos de filtros

### Todos as mensagens serão enviadas, menos as que tiverem o termo `política`

Conteúdo de `RULES.txt`:

```
ACCEPT:ALL
DROP:Política
```

### Nenhuma mensagem será enviada, com exceção das mensagens com os termos `futebol` e `vôlei`

Conteúdo de `RULES.txt`:

```
DROP:ALL
ACCEPT:futebol
ACCEPT:vôlei
```

## Observações

1. Bot somente envia mensagem para pessoas que já falaram com ele. Ou seja, é necessário que a pessoa tenha clicado ao menos uma vez em `Iniciar` para que o bot consiga enviar uma mensagem.

2. Bot somente entra em canais como administrador.

## Participe

Entre na aba [Discussions](https://github.com/GabrielRF/Rss2Telegram/discussions) no GitHub e vamos conversar sobre o projeto!
