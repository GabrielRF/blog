---
layout: post
title: "RastreioBot - Primeiro milhão de pacotes"
date: 2021-10-20 12:30:00 -0300
categories: [Telegram, Bot]
tags: telegram bot rastreiobot
image: "https://rastreiobot.xyz/images/rastreiobot.png"
---

> [https://rastreiobot.xyz](https://rastreiobot.xyz)

<center>
<img src="https://rastreiobot.xyz/images/rastreiobot.png" alt="Logo RastreioBot." style="width:50%; border-radius:50%"> 
<br><small>Logo RastreioBot.</small>
</center>

Criado em 03 de Julho de 2015, o RastreioBot precisou de 2295 dias para alcançar a marca de 1.000.000 pacotes rastreados. Aproximadamente 400.000 pacotes foram rastreados só nos últimos seis meses![^1]

Muito obrigado a todas as pessoas que me ajudaram com o bot, seja fazendo comentários, críticas, ajudando com a programação ou até com doações. A soma de tudo isto trouxe o bot ao primeiro milhão de pacotes.

[https://telegram.me/RastreioBot](https://telegram.me/RastreioBot)

## História

<center>
<img src="/assets/img/rastreiobot_old.jpeg" alt="Logo antigo do RastreioBot." style="width:50%; border-radius:50%"> 
<br><small>Logo antigo do RastreioBot.</small>
</center>

### Idéia inicial

O bot foi criado com um único objetivo, reduzir consultas no site dos Correios.

A primeira versão era bastante simples. Não tinha banco de dados nem enviava alertas. Tudo o que o bot fazia era salvar a última mensagem recebida em um arquivo de texto `.txt` e exibir um botão `/Repetir` que, quando clicado, retornava o status disponível no sistema dos Correios. Ou seja, se a pessoa tivesse mais de um pacote, além de o bot ter salvo somente o último enviado, o bot não enviava nenhum alerta a menos que fosse consultado.

O bot rodava em um Raspberry Pi 3, em casa mesmo, onde ficou durante muitos anos.

<center>
<img src="/assets/img/rack_caseiro.jpg" alt="Pilha de Raspberry Pi." style="width:50%"> 
<br><small>Pilha de Raspberry Pi.</small>
</center>

### Evolução

Somente em Feveiro de 2017 o bot começou a engatinhar para a versão atual. Foi aberto um [novo repositório](https://github.com/GabrielRF/RastreioBot) e foi criado o primeiro banco de dados. Principais novidades:
- Uso de banco de dados;
- Alertas de movimentações;
- Possibilidade de descrever os pacotes.

Rapidamente as avaliações do bot melhoraram e ele começou a aparecer entre os 10 melhores (em português) no antigo sistema de votação de bots, o StoreBot. Foi nesta versão também que comecei a receber as colaborações de código do meu amigo [Marco Rougeth](https://twitter.com/marcorougeth), que até hoje me salva com o Python.

<center>
<img src="/assets/img/rastreiobot_top10_storebot.jpeg" alt="Site do StoreBot." style="width:50%"> 
<br><small>Site do StoreBot.</small>
</center>

### Limite de pacotes

Em abril de 2017 o bot atingiu a quantidade de 100 pacotes rastreados simultaneamente. Neste momento o site dos Correios passou a bloquear meu IP, pois eram muitas consultas. Quando o IP era bloqueado, demorava demais a voltar a funcionar. Migrei então para a API oficial dos Correios[^2], que é usada até hoje.

### 2018

Em 2018 passei a receber mais colaborações com o código do bot, recebendo ajuda de diversas pessoas. Dentre elas, mais um amigo que me ajuda até hoje, o [Bigua](https://twitter.com/otalbigua). Foi com a ajuda dele que começamos a fazer *sprints* do bot, rapidamente reescrevendo o que precisava de mudanças. Foi ele também que começou a organizar o código para facilitar a integração de outras plataformas, dando início ao uso da TrackingMore, plataforma para rastrear pacotes fora do Brasil. No mesmo ano participei da PyCon[^3], apresentando um poster e contando um pouco da história do bot.

<center>
<img src="/assets/img/pycon2018.jpeg" alt="Participação na PyCon 2018." style="width:50%"> 
<br><small><a href="https://us.pycon.org/2018/speaker/profile/556/">Participação na PyCon 2018.</a></small>
</center>

Foi neste ano também que recebi de presente a atual arte do bot. Obrigado, Pedro Hartmann.


### Tá pegando fogo, bicho

Um incêndio em uma subestação da Companhia Energética de Brasília (CEB) em 2019 fez o Raspberry Pi do bot desligar abruptamente, corrompendo o cartão de memória[^4]. Ao tentar restaurar o backup, notei que o backup estava sendo feito incorretamente. Infelizmente, o banco de dados do bot foi perdido. Para evitar futuros problemas, neste momento o bot foi movido para a nuvem, para uma máquina virtual na ScaleWay. O backup foi corrigido e mantido na Amazon, em um *bucket* S3.

<center>
<img src="/assets/img/fogo_ceb.gif" alt="Fogo em subestação da CEB. Imagem da CBN Brasília." style="width:50%"> 
<br><small><a href="https://twitter.com/cbnbrasilia/status/1133713747632152577?s=20">Fogo em subestação da CEB. Imagem da CBN Brasília.</a></small>
</center>

### O bot e a pandemia

O bot não para de crescer. Com a pandemia, as compras online aumentaram bastante e, consequentemente, o uso do bot explodiu. Em 2021 o bot precisava verificar mais de 75.000 pacotes de uma vez só, precisando de quase uma hora para concluir a rotina. Com a ajuda do Rougeth, esse número foi reduzido para incríveis três minutos, otimizando as verificações assíncronas e diminuindo o atraso entre a movimentação do pacote e o envio da mensagem para a pessoa.

### Primeiro milhão

No dia 13 de Outubro de 2021, pela manhã, o bot atingiu a marca de 1.000.000 pacotes rastreados desde a sua criação. O bot foi construído em Python e as pessoas da comunidade Python Brasil são grandes responsáveis pelos avanços na programação do bot. Alcançar esse recorde durante a [Python Brasil 2021](https://2021.pythonbrasil.org.br/) foi muito simbólico, tendo sido registrado mais tarde em uma [palestra relâmpago](https://youtu.be/-OMV8DEEN8E) durante o evento.

Muito obrigado a todas as pessoas que me ajudaram e me ajudam!

E vamos ao segundo milhão! 🚀

---

[^1]: Diariamente os status do bot são postados no Twitter. [@RastreioBot](https://twitter.com/RastreioBot)
[^2]: API dos Correios: [https://www.correios.com.br/atendimento/ferramentas/sistemas/arquivos/web-service-de-rastreamento](https://www.correios.com.br/atendimento/ferramentas/sistemas/arquivos/web-service-de-rastreamento)
[^3]: Maior conferência de Python no mundo, ocorre anualmente. [https://pycon.org/](https://pycon.org/)
[^4]: Post no Medium sobre o problema: [https://medium.com/@gabrielrf/socorro-meus-pacotes-sumiram-e6ecf6b153ab](https://medium.com/@gabrielrf/socorro-meus-pacotes-sumiram-e6ecf6b153ab)
