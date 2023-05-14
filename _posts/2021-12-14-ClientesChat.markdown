---
layout: post
title: "Atendimento a clientes usando Telegram"
date: 2021-12-14 12:00:00 -0300
categories: [Telegram, Bot]
tags: telegram bot clienteschatbot sac atendimento
image: "/assets/img/ClientesChat_canal.png"
---

> Como ter uma equipe de atendimento e manter organizadas as conversas com os clientes?

Uma solução de código aberto que auxilia no atendimento e na organização das conversas com seus clientes.

O serviço é composto por três partes:

* `Bot`: Por onde as pessoas entram em contato. Do ponto de vista do usuário, a conversa é sempre feita via bot.
* `Canal`: A equipe de atendimento faz tudo pelo canal, usando os comentários.
* `Grupo`: Serve para ativar os comentários no canal e tem também a funcionalidade de ser um log de tudo.

Em resumo: As pessoas (clientes) devem interagir com o bot e a equipe de atendimento com o canal, usando os comentários. O grupo é limitado a quem o criou e ao bot apenas.

## Atendimento

*Do ponto de vista da equipe de atendimento*, as conversas ficam separadas em tópicos no canal. Diferentes pessoas podem fazer o atendimento simultaneamente, sendo transparente para quem está sendo atendido.

*Já para quem recebe o atendimento*, tudo vem com o nome e a marca da empresa, pois a conversa é feita através do bot.

## Edição e Remoção de Conteúdo

*A conversa não some*. A única possibilidade de uma tela ser limpa é se alguém *manualmente* apagar algo. Porém, a exclusão de conteúdo *ocorre somente do lado que a executou*. Ou seja, se um atendente remove a conversa, quem é atendido continua com todo o histórico perfeito. Se a pessoa excluiu ou bloqueou o bot, a conversa também permanece intacta do lado da equipe de atendimento.

As mensagens editadas são editadas *dos dois lados*. Uma mensagem editada pela pessoa atendida é alterada também na visão da equipe de atendimento e uma mensagem editada pela equipe também é alterada na visão do cliente[^1]. 

## Dados Pessoais

De acordo com o funcionamento do Telegram, as informações visíveis ao bot são apenas `nome`, `sobrenome` e `ID`, dados que são exibidos para a equipe de atendimento assim que a pessoa envia a primeira mensagem ao bot. Nenhuma outra informação é coletada automaticamente. Qualquer outro dado deve ser solicitado durante a conversa.

## Respostas Prontas

Pensando na agilidade do atendimento, há um módulo de *Respostas Prontas* presente no bot! As respostas prontas funcionam em *modo inline*. Ou seja, a *equipe de atendimento*[^2] pode usá-las em qualquer chat, basta escrever no campo de envio de mensagens o `@usuario` do bot e selecionar a resposta a ser enviada. Os termos escritos após `@usuario` servem para filtrar respostas, exibindo apenas as respostas que contém o argumento desejado.

<center>
<img src="https://clientes.chat/images/resposta_uso.png" alt="Uso de Respostas Prontas" style="width:50%"> 
<br><small>Uso de Respostas Prontas</small>
</center>

## Mais informações

Visite o site [Clientes.Chat](https://clientes.chat)

---

[^1]: Seguindo as regras do próprio Telegram, a edição pode ocorrer somente nas primeiras 48 horas após o envio da mensagem. [Telegram.org](https://telegram.org/blog/unsend-privacy-emoji/?setln=pt-br#unsend-anything)

[^2]: As Respotas Prontas somente funcionam para a equipe de atendimento. Outras pessoas recebem como resposta um link para receber atendimento.
