---
layout: post
title: "Como obter IDs no Telegram"
date: 2021-07-27 13:30:00 -0300
categories: [Telegram, Curiosidades]
tags: telegram bot
image: "/assets/img/TelegramGlasses.jpeg"
---

> Post para desenvolvedores.

## Antes de mais nada...

* **Pessoas**: Sempre com ID positivo;

* **Canais**: Sempre com ID negativo;

* **Grupos**: Sempre com ID negativo. Caso seja um supergrupo[^1], comecará com `-100`.

## Telegram Web

Este método sempre funciona, não importa qual seja o chat.

Abra as informações do chat, não importa se é pessoa, grupo ou canal.


<center>
<img src="/assets/img/TelegramID_1.png" alt="Perfil do DicasChat" style="width:50%"> 
<br><small>Perfil do DicasChat.</small>
</center>

Use o inspetor de código e encontre a div `<div class="profile-name">`.

<center>
<img src="/assets/img/TelegramID_2.png" alt="Código HTML do Telegram Web" style="width:50%"> 
<br><small>Código HTML do Telegram Web.</small>
</center>
O ID estará presente em `data-peer-id`.

Como sei que o grupo do exemplo é um supergrupo[^1], o ID correto é `-1001055895627`.

## Usando seu próprio bot

Para este método funcionar, é necessário que a pessoa fale com o bot ou que o grupo faça parte do grupo/canal em que se deseja obter o ID.

Acesse o endereço `https://api.telegram.org/bot158700146:AAHOPReqqTR8V7FXysa8mJCbQACUWSTBog8/getUpdates`, colocando o `token` do seu bot.

Caso queira o ID de um grupo ou canal, basta verificar em `chat` `id`. No exemplo, o canal @PromoPassagens tem o ID `-1001002634335`.

<center>
<img src="/assets/img/TelegramID_3.png" alt="JSON de um bot exibindo o chat.id." style="width:50%"> 
<br><small>JSON de um bot exibindo chat.id.</small>
</center>

Caso esteja buscando o ID de uma pessoa, basta verificar em `from` `id`. O meu ID, como mostrado no exemplo, é `9083329`.

<center>
<img src="/assets/img/TelegramID_4.png" alt="JSON de um bot exibindo o from.id" style="width:50%"> 
<br><small>JSON de um bot exibindo from.id.</small>
</center>

## Usando bots no Telegram

> **Não funcionam para**:
> * Pessoas com a opção de privacidade em mensagens encaminhas ligada;
> * Grupos privados;
> * Canais privados.

### [@ShowJsonBot](https://t.me/ShowJsonBot)

Encaminhe a mensagem da pessoa, do grupo ou do canal para o bot e veja na resposta o ID. No caso, a mensagem encaminhada foi do canal @DicasTelegram, cujo ID é `-1001029093254`. A resposta do bot é bem completa, mostrando o ID de quem enviou a mensagem para o bot, no caso `9083329`, e mais algumas informações que podem ser úteis. 

<center>
<img src="/assets/img/TelegramID_5.png" alt="Show Json Bot" style="width:50%"> 
<br><small>Show Json Bot</small>
</center>

### [@UserInfoBot](https://t.me/UserInfoBot)

Encaminhe a mensagem da pessoa, grupo ou canal para o bot e ele responderá com o ID.

<center>
<img src="/assets/img/TelegramID_6.png" alt="userinfobot" style="width:50%"> 
<br><small>userinfobot</small>
</center>

[^1]: O Telegram deixou transparente para o usuário a mudança de grupo para supergrupo. Para ler mais, <a href="https://dicastelegram.com.br/2020/11/18/convertendo-grupos-comuns-em-supergrupos/">clique aqui</a>.

[^2]: Não funciona para: Pessoas com a opção de privacidade em mensagens encaminhas ligada; Grupos privados; Canais privados.
