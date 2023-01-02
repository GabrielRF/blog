---
layout: post
title: "Send2KindleBot - Mudanças recentes na Amazon"
date: 2023-01-02 18:00:00 -0300
categories: Telegram Bot
tags: kindle send2kindlebot telegram bot amazon
image: "https://github.com/GabrielRF/Send2KindleBot/raw/master/icon.png?raw=true"
---

> Porque seus arquivos não chegam

<center>
<img src="https://github.com/GabrielRF/Send2KindleBot/raw/master/icon.png?raw=true" alt="Send to Kindle Bot Logo" style="width:50%; border-radius:50%">
<br><small>Logo do Send to Kindle Bot</small>
</center>

## Funcionamento

O [@Send2KindleBot](https://t.me/Send2KindleBot) pede dois e-mails assim que a conversa é iniciada. O e-mail usado em sua conta Amazon e o e-mail de seu dispositivo/app Kindle.

### Por que?

Porque o bot utiliza o seu e-mail para enviar o documento. Ou seja, o bot "preenche" o campo remetente com o seu endereço de e-mail da conta e o de destinatário com o do kindle. Isto facilita o uso do bot e também a configuração do servidor de e-mail que o bot utiliza.

### O que mudou?

Recentemente, a Amazon passou a verificar o DNS reverso do e-mail, o que faz sentido. A consequência disso é que agora, ao receber um documento, a Amazon consulta se o servidor que preencheu o campo de remetente de fato é o servidor daquele endereço. Ou seja, a Amazon verifica se o servidor do bot é o servidor do GMail, do Hotmail, do Yahoo etc. Obviamente não é, falhando na verificação. Assim, o e-mail enviado pelo bot cai no que seria uma caixa de SPAM, exigindo que o usuário precise confirmar o envio clicando em um link que chega por e-mail, no mesmo endereço usado na conta da Amazon.

<center>
<img src="/assets/img/send2kindle_verify.png" alt="E-mail de verificação de documento enviado" style="width:50%; border-radius:5%"> 
<br><small>E-mail de verificação de documento enviado</small>
</center>

## Correção

Infelizmente a correção é inviável, portanto, não será feita. As possibilidades são as seguintes:

### Domínio próprio

Uma possível solução seria criar um domínio para o bot. O problema é que para dar certo, eu teria dois custos adicionais.

* Um servidor próprio para o bot, com IP público;

* Um nome de domínio para o bot.

Para quem usa o bot seria necessária a autorização de um novo endereço de e-mail com permissão de envios. Ou seja, dificultaria o uso.

Além disso, aumentaria muito o custo do bot, pois tanto o servidor quanto o domínio são pagos. Isto tornaria o bot inviável de ser mantido. Seu custo mais que dobraria.

### E-mail próprio

Outra possibilidade seria ter um e-mail exclusivo para o bot, exemplo `send2kindlebot@hotmail.com`. Além de ter o mesmo problema de exigir a configuração de cada usuário, também seria criada uma brecha de segurança.

Parte da segurança do envio de documentos existe por exigir a combinação perfeita entre remetente e destinatário. Tendo um mesmo remetente para todas as pessoas, alguém com más intenções teria 50% menos trabalho para fazer SPAM, pois bastaria usar o mesmo e-mail que o bot para disparar documentos para as pessoas. Não me parece uma boa idéia.

### Solução ideal

Seria muito interessante se a Amazon confiasse no servidor usado pelo bot. A configuração a ser feita seria simples e resolveria o problema por completo. Houve uma tentativa de contato com a empresa em 16/11/2022, mas sem respostas.

### Solução real

Por enquanto, não há o que ser feito. Todos os envios vão exigir a verificação por e-mail enquanto não surgir uma idéia melhor.

Atualizarei o post quando houver uma resposta da Amazon.
