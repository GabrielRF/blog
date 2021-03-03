---
layout: post
title:  "O RastreioBot apagou minha mensagem!"
date:   2021-03-03 15:00:00 -0300
categories: [Telegram, Bot]
tags: telegram segurança bot rastreiobot
---

É comum alguém comentar que o RastreioBot apagou uma mensagem que não era código de rastreio. Mas calma, tudo tem explicação. 

O RastreioBot é um programa de computador que funciona 24 horas por dia em uma máquina na nuvem. Ele segue um algoritmo que recebe as mensagens e verifica automaticamente se o pacote foi atualizado no sistema dos Correios. Seu papel é unicamente este. 

Porém, por diversos motivos, algumas pessoas têm o hábito de usar o bot como bloco de notas, local para salvar fotos, textos e muito mais, fugindo totalmente de seu escopo de funcionamento. **Não recomendo fazer isto!**

<center>
<img src="/assets/img/botmsgdel.gif" alt="Send to Kindle Logo" style="width:50%"> 
<br><small>Mensagem sendo apagada no RastreioBot</small>
</center>

Todo bot do Telegram é controlando pelo *token*, uma chave fornecida pelo app assim que o bot é cadastrado. O *token* deve ser mantido em segredo, pois é com ele que o bot é controlado e as interações são feitas. **Uma pessoa em poder do *token*, sendo quem criou o bot ou não, é capaz de ver todo o histórico do bot**, tudo o que já passou por ele, sejam mensagens diretamente ao bot ou mensagens em grupos em que o bot possa ler tudo. 

Quando uma mensagem é apagada pelo bot, **a mensagem some para o usuário e também dos servidores do Telegram**. Ou seja, ao apagar a mensagem, todo o risco de vazamento de dados acaba, pois a mensagem deixa de existir. Mesmo que ocorra um problema em que o *token* fique exposto ou se torne público, as mensagens apagadas não poderão ser recuperadas.

Portanto, não se preocupe. Sua mensagem foi apagada para o seu bem. A mensagem some do programa do bot, do servidor do Telegram e da sua tela. A informação some da onde não devia estar e continua sendo exclusivamente sua.  

## Mensagens salvas

O Telegram oferece um local específico para salvar tudo que você quiser, é o espaço *Mensagens Salvas*. Lá você pode salvar textos e arquivos de até 2GB cada, sem limite de arquivos. Para acessar, vá nas configurações do app e clique em *Mensagens Salvas*.

**Dica: Fixe o *Mensagens Salvas* no topo das conversas.**
