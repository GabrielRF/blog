---
layout: post
title: "Feed RSS no Telegram sem precisar de servidor"
date: 2022-07-14 20:50:00 -0300
categories: [Telegram, Bot]
tags: telegram bot canal automação github rss
image: "/assets/img/rsslogo.png"
---

<center>
<img src="/assets/img/rsslogo.png" alt="Logo RSS" style="width:50%; border-radius:20%"> 
<br><small>Logo RSS</small>
</center>

Envio automático de feed RSS no Telegram! Funciona com envios para pessoas, grupos e canais, públicos ou privados.

## Configuração

Faça um fork do [repositório original](https://github.com/GabrielRF/Rss2Telegram), clicando em `fork`.

Vá em `Settings`, `Secrets`, `Actions` e crie as seguintes variáveis:

### Obrigatórias

* `BOT_TOKEN`: Token do bot que fará os envios. Fale com o [@BotFather](https://t.me/BotFather) caso não tenha um;

* `DESTINATION`: IDs ou `@` de quem receberá as mensagens;

* `URL`: Lista de urls dos feeds separados por vírgulas.

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

### Ação

Ajustadas as variáveis, vá em `Actions`, `Select workflow` e clique em `Enable workflow`. A ação irá rodar a cada 1 hora para verificar as atualizações.

Para alterar o intervalo da ação basta alterar o arquivo `.github/workflows/cron.yml`.

Exemplos:

`- cron: '0 */1 * * *'`: Faz a ação ser executada uma vez por hora;

`- cron: '*/15 * * * *'`: Faz a ação ser executada uma vez a cada quinze minutos.

## Exemplos

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

## Observações

1. Bot somente envia mensagem para pessoas que já falaram com ele. Ou seja, é necessário que a pessoa tenha clicado ao menos uma vez em `Iniciar` para que o bot consiga enviar uma mensagem.

2. Bot somente entra em canais como administrador.


