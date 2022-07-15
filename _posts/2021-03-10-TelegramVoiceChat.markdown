---
layout: post
title:  "ChatDeVozBot - Inserções de voz no Telegram"
date:   2021-03-10 12:00:00 -0300
categories: [Telegram, VoiceMeeter Banana]
tags: telegram voicemeeter banana voz audio
image: "https://blog.gabrf.com/assets/img/BananaScreen.png"
---

> Atenção: Para este guiar funcionar, é necessário ler [este post](https://blog.gabrf.com/posts/TelegramBanana/) antes!

<center>
<img src="/assets/img/BananaScreen.png" alt="Tela inicial do VoiceMeeter Banana" style="width:100%"> 
<br><small>Tela inicial do VoiceMeeter Banana</small>
</center>

Dando continuidade ao uso do VoiceMeeter, criei um bot para ser usado em Chats de Voz no Telegram. Sua principal função é receber mensagens de áudio e as encaminhar para uma pessoa responsável pela análise e reprodução do conteúdo. 

## Instruções para uso no grupo

Adicione o [ChatDeVozBot](https://t.me/ChatDeVozBot) como administrador de seu grupo.

### Iniciar o bot

Para iniciar o bot no grupo, envie `/iniciar`. O bot enviará uma mensagem comunicando o início de seu funcionamento e fixará uma mensagem que contém um botão para que os áudios sejam enviados.

> A pessoa que enviar este comando receberá todos os áudios em privado, via bot, para análise do conteúdo.

<center>
<img src="/assets/img/voicechatstart.gif" alt="Iniciando o ChatDeVozBot no grupo" style="width:50%"> 
<br><small>Iniciando o ChatDeVozBot no grupo</small>
</center>

### Como usar o VoiceMeeter

A pessoa que administrará os áudios fará o controle do que será ou não tocado no chat de voz nos botões do VoiceMeeter. Neste caso, a parte mais importante é o <b>retângulo amarelo</b>, a *Entrada Virtual*. Para este uso em específico, recomendo que o volume seja deixado em 0dB.

<center>
<img src="/assets/img/BananaComplete.png" alt="VoiceMeeter Banana pronto para uso" style="width:100%"> 
<br><small>VoiceMeeter Banana pronto para uso</small>
</center>

Para que **somente a pessoa que administra a fila ouça o áudio**, basta desativar `B1` no retângulo amarelo.

Deixando `B1` ativado, **o áudio será enviado para o Chat de Voz**, permitindo que todos o ouçam.

### Parar o bot

Para encerrar, basta que qualquer administrador envie `/parar`.

<center>
<img src="/assets/img/voicechatstop.gif" alt="Parando o ChatDeVozBot no grupo" style="width:50%"> 
<br><small>Parando o ChatDeVozBot no grupo</small>
</center>

## Instruções para uso geral

Clique no botão `/Participar` enviado no grupo. Será aberta uma conversa com o bot. Clique em `Começar` e, em seguida, envie sua mensagem de voz.

<center>
<img src="/assets/img/voicechatstartuser.gif" alt="Enviando mensagem de voz para o ChatDeVozBot" style="width:50%"> 
<br><small>Enviando mensagem de voz para o ChatDeVozBot</small>
</center>

Para enviar uma nova mensagem, repita o procedimento. 
