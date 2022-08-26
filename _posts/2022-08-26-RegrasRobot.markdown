---
layout: post
title: "Bot de envio de regras e controle de acesso a grupos"
date: 2022-08-26 18:00:00 -0300
categories: [Telegram, Bot]
tags: telegram grupo bot regras
image: "/assets/img/regrasrobot_logo.jpg"
---

<center>
<img src="/assets/img/regrasrobot_logo.jpg" alt="Logo RegrasRobot" style="width:50%; border-radius:50%"> 
<br><small>Logo RegrasRobot</small>
</center>

Este bot tem uma função muito simples e objetiva, permitir a entrada de pessoas em um grupo do Telegram somente após lerem e aceitarem as regras. Ele pode ser usado em grupos públicos ou privados e sua configuração deve ser feita pessoa pessoa que é dona do grupo. O bot não aceitará fazer a configuração por outra pessoa.

## Como usar

- Adicione o bot ao seu grupo;

- Transforme seu grupo em supergrupo. [Leia mais informações aqui](#grupo-vs-supergrupo);

- Adicione o bot ao grupo e coloque-o como administrador;

- No grupo, envie o comando `/set_rules` e defina as regras. A mensagem enviada para a pessoa será exatamente a mesma enviada neste etapa.

### Grupos públicos

Nas opções do grupo, vá em tipo de grupo e ligue a opção `Aprovar novos membros`. Pronto. Isto é suficiente para que o bot entre em contato com as pessoas quando elas tentarem entrar no grupo e envie a mensagem com as regras.

### Grupos privados

Em grupos privados, o bot somente irá funcionar para as pessoas que entrarem no grupo usando um link com a opção `Pedir aprovação de admins` ligada. Para gerar um link com esta opção, basta abrir os ajustes do grupo e clicar em `Links de convite`. Gere um link com a opção ligada e envie para as pessoas.

### Regras do grupo

As regras do grupo podem ser acessadas enviando-se o comando `/regras` no grupo. A mensagem com o comando será automaticamente apagada e enviada como mensagem privada. Caso o bot não consiga enviar a mensagem para a pessoa, uma mensagem será enviada no grupo com um link que permitirá o envio em privado assim que clicado. Esta mensagem enviada no grupo será apagada automaticamente em alguns segundos, mantendo o grupo sempre organizado.

### Captcha

Ao solicitar a entrada em um grupo, para confirmar que leu e entendeu as regras será necessário que a pessoa clique no botão correspondente ao emoji descrito na mensagem. A idéia é que funcione como um captcha, barrando contas automatizadas de entrarem no grupo. A mensagem com o captcha fica disponível por dois minutos. Caso não seja respondida ou seja respondida incorretamente, a mensagem é automaticamente removida.

## GitHub

O bot é de código aberto e toda colaboração é bem vinda! [Visite o repositório aqui](https://github.com/GabrielRF/RegrasRobot).

## Grupo vs Supergrupo

Inicialmente criamos grupos. Com a evolução do app, o Telegram criou o conceito de supergrupos. Para o usuário, isto faz pouca ou nenhuma diferença. Para o bot isto é importante pois altera o ID do grupo, que é a parte importante quando tratamos de bots. Para não haver confusões futuras, o bot somente funciona em supergrupos.

A transformação de grupo em supergrupo é feita de maneira automática pelo Telegram. A maneira mais fácil e imediata de forçar tal mudança é alterando as regras de visibilidade de histórico do grupo. Abra as opções do grupo e altere para deixar o histórico vísivel sempre. Ao fazer isto, automaticamente o grupo é transformado em supergrupo e isto não pode ser desfeito, independente do ajuste escolhido em sequência.
