---
layout: post
title:  "Send2KindleBot - Como enviar arquivos para seu Kindle"
date:   2021-02-25 15:00:00 -0300
categories: Telegram Bot
tags: kindle send2kindlebot telegram bot
---
O [Send2KindleBot](https://t.me/Send2KindleBot) é a maneira mais prática de enviar arquivos e documentos para seu Kindle! É um bot no [Telegram](https://telegram.org), gratuito e de [código aberto](https://github.com/GabrielRF/Send2KindleBot), traduzido para mais de 20 idiomas. 

<center>
<img src="https://gabrf.com/images/send2kindlebot.png" alt="Send to Kindle Logo" style="width:50%; border-radius:50%"> 
<br><small>Logo do Send to Kindle</small>
</center>

[Instruções de uso](#instruções-de-uso)

[Soluções de problemas](#soluções-de-problemas)

#### Instruções de uso

* Usando o Telegram, fale com o [@Send2KindleBot](https://t.me/Send2KindleBot)
* Clique em `Começar`
* Conforme solicitado, envie o e-mail de seu dispositivo Kindle, que normalmente termina em `@kindle.com`
* Em seguida, envie o e-mail de sua conta da Amazon. É o seu e-mail usado para acessar o site da Amazon (GMail, Hotmail etc)
* Pronto! 🎉

Feita a configuração, o bot estará pronto para enviar os arquivos, sejam eles mobi, doc, txt, epub ou outro formato. O bot também é capaz de enviar sites para o Kindle. Neste caso, um PDF da página será enviado para seu dispositivo, sendo possível optar no momento do envio se uma conversão para mobi deve ser feita ou não.

#### Soluções de problemas

> **Bot não funciona ou os arquivos não chegam**

Envie `/start` para o bot e confira se os e-mails estão certos. Qualquer erro de digitação faz diferença. Para corrigir o problema, clique no botão `Ajustar e-mail` e envie o endereço que deseja alterar, sendo o do Kindle ou o da sua conta. 

Se não houver erro, verifique se chegou algo em seu e-mail. A solução abaixo pode te ajudar se for este o caso.

> **Amazon envia e-mail pedindo confirmação do envio**

A confirmação de envio é necessária quando os dois e-mails (do Kindle e da conta) são muito parecidos. Exemplo fictício:

`gabriel@gmail.com` e `gabriel@kindle.com`

Neste caso, para corrigir o problema, basta deixá-los diferentes. Ou seja, trocar o e-mail do Kindle ou o e-mail autorizado a enviar documentos.

Basta seguir **uma** das instruções abaixo para resolver o problema.

> **Trocar o e-mail do Kindle**

Acesse o site da Amazon. Vá em `Conta`, `Dipositivos`, Selecione o Kindle, Clique em `Editar` para alterar o e-mail. Algo como `kindlegabriel@kindle.com` seria um exemplo de solução, pois deixaria bem diferente de `gabriel@gmail.com`. Feita a alteração, volte ao bot, clique em `Alterar e-mail` e envie o novo e-mail cadastrado. 

<center>
<img src="/assets/img/send2kindle_kindleemail.gif" alt="Instruções de troca de e-mail do Kindle" style="width:50%"> 
<br><small>Instruções de troca de e-mail do Kindle</small>
</center>

> **Adicionar um e-mail autorizado a enviar arquivos**

Acesse o site da Amazon, vá em `Conta`, `Dispositivos`, `Preferências`, `Configurações de documentos pessoais`, `Adicionar um novo endereço de e-mail aprovado` e adicione um outro e-mail que você tenha. Por fim, clique em `Alterar e-mail` no bot e envie o novo e-mail cadastrado.

<center>
<img src="/assets/img/send2kindle_accountemail.gif" alt="Instruções de troca de e-mail autorizado" style="width:50%"> 
<br><small>Instruções de troca de e-mail autorizado</small>
</center>
