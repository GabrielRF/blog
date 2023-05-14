---
layout: post
title:  "VoiceMeeter Banana e Chat de Voz do Telegram"
date:   2021-03-07 12:00:00 -0300
categories: [Telegram, VoiceMeeter Banana]
tags: telegram voicemeeter banana voz audio
image: "/assets/img/BananaComplete.png"
---
*[Originalmente postado no Telegraph](https://telegra.ph/VoiceMeeter-Banana-e-Chat-de-voz-do-Telegram-01-28)*

---

> Este será um breve guia de como controlar o que é transmitido para o Telegram. Não será um guia altamente técnico nem detalhado. A intenção é apenas demonstrar, de maneira rápida, como controlar o som de seu computador.

## Instalação

Este software funciona somente em **Windows** e pode ser [baixado aqui](https://voicemeeter.com/). Faça o download e siga os passos da instalação. É recomendável reiniciar o computador no fim da instalação.

## Configuração de saídas

O programa pode ser visto como dividido em duas partes. Inicialmente daremos atenção ao lado direito do programa, onde são configuradas as interfaces de **saída de som**.

<center>
<img src="/assets/img/BananaOutputs.png" alt="Saídas de som do VoiceMeeter" style="width:100%"> 
<br><small>Saídas de som do VoiceMeeter</small>
</center>

Inicie os ajustes pelas saídas físicas do programa, marcadas em vermelho. Coloque ali tudo o que você usar para escutar o som, podendo ser o próprio monitor, fones de ouvido, caixas de som etc. O programa suporta no máximo três saídas de som físicas. Para funcionar é necessário ao menos uma. 

Na imagem de exemplo estão configuradas duas interfaces de saída, sendo:

* `A1`: Fone de ouvido
* `A2`: Monitor
* `A3`: Não utilizada

Recomendo nomear os ajustes de nível de maneira a facilitar o controle. Para isto, basta clicar com o botão direito do mouse no nome que deseja alterar, como demonstrado no quadrado amarelo da imagem. Repare que ali aparecem mais duas saídas, as saídas virtuais, sendo elas:

* `B1`: Saída virtual principal
* `B2`: Saída virtual auxiliar

O controle de volume de todas as saídas é feito movendo-se as barras verdes. Zero dB significa o volume "normal", sem amplificação. Ou seja, caso seu fone, por exemplo, tenha um controle de volume, zero dB será igual ao ajustado no fone. Feita uma alteração no valor, para voltar ao zero basta um duplo clique na barra verde. 

Valores acima de zero podem causar distorções no som e até problemas por excesso de amplificação. Sugiro nunca passar de zero. Se precisar aumentar o volume, sugiro controlar no próprio dispositivo.

## Configurações de entrada


<center>
<img src="/assets/img/BananaInputs.png" alt="Entradas de som do VoiceMeeter" style="width:100%"> 
<br><small>Entradas de som do VoiceMeeter</small>
</center>

Já no lado esquerdo do programa são feitos os ajustes das interfaces de entrada. Na parte em vermelho, clique nos textos Hardware Input e defina quais são seus componentes de entrada. Na imagem de exemplo está definido apenas uma interface de entrada, um microfone. Na imagem temos:

* `Hardware Input 1`: Microfone
* `Hardware Input 2`: Não utilizada
* `Hardware Input 3`: Não utilizada

No quadrado amarelo são definidos os volumes das entradas. Ali estão definidas também duas interfaces virtuais de entrada, localizadas na porção mais a direita dentro retângulo amarelo.

## Configurações no Windows

Clique com o botão direito do mouse sobre o ícone de som e selecione Abrir Configurações de Som.

<center>
<img src="/assets/img/BananaWindowsSetInt.png" alt="Opções de som do Windows" style="width:50%"> 
<br><small>Opções de som do Windows</small>
</center>

Será aberta a janela de configurações do Windows.

<center>
<img src="/assets/img/BananaWindowsInt.png" alt="Configurações de som do Windows" style="width:70%"> 
<br><small>Configurações de som do Windows</small>
</center>

Selecione a entrada virtual principal do VoiceMeeter como saída de som e VoiceMeeter Output como a entrada de som, conforme a imagem. Ou seja, o som que sair do Windows entrará no VoiceMeeter e o som que entrar no Windows terá como origem o VoiceMeeter. 

## Configurações no Telegram

Entre em um chat de voz do Telegram e clique nos ajustes de som, marcados em vermelho.

<center>
<img src="/assets/img/BananaVoiceChat.png" alt="Canal de voz do Telegram" style="width:50%"> 
<br><small>Canal de voz do Telegram</small>
</center>

Será aberta a tela de ajustes. Configure como a imagem, com

* `Speakers`: Entrada virtual auxiliar do VoiceMeeter
* `Microphone`: Saída do VoiceMeeter

<center>
<img src="/assets/img/BananaVoiceChatSettings.png" alt="Ajustes de som do canal de voz " style="width:50%"> 
<br><small>Ajustes de som do canal de voz</small>
</center>

## Utilização

Feita a configuração, temos:

* `Entrada 1`: Microfone
* `Entrada Virtual Principal`: Windows
* `Entrada Virtual Auxiliar`: Chat de Voz
* `Saída A1`: Fone de Ouvido
* `Saída A2`: Monitor
* `Saída B1`: Saída de Som Virtual

Desta forma, para fazer com que os participantes ouçam o microfone basta selecionar B1 no microfone.


<center>
<img src="/assets/img/BananaB1.png" alt="VoiceMeeter enviando o som do microfone para a saída B1" style="width:100%"> 
<br><small>VoiceMeeter enviando o som do microfone para a saída B1</small>
</center>

Para que o som do chat de voz saia no fone de ouvido, basta selecionar A1 na Entrada Virtual Auxiliar.

<center>
<img src="/assets/img/BananaA1.png" alt="VoiceMeeter enviando som do chat de voz para o fone de ouvido, saída A1" style="width:100%"> 
<br><small>VoiceMeeter enviando som do chat de voz para o fone de ouvido, saída A1</small>
</center>

## Configuração completa

<center>
<img src="/assets/img/BananaComplete.png" alt="VoiceMeeter corretamente configurado para o Chat de Voz com música" style="width:100%"> 
<br><small>VoiceMeeter corretamente configurado para o Chat de Voz com música</small>
</center>

Finalmente, esta é a forma correta de se configurar o VoiceMeeter para que se use o Chat de Voz do Telegram transmitindo música.

### Reângulo Vermelho

Microfone enviando o som para B1, permitindo que os participantes do Chat de Voz escutem o que é falado.

### Retângulo Amarelo

Entrada virtual enviando som para A1 e B1, ou seja, a música que é tocada no meu Windows (seja no navegador, no Spotify etc) é enviada para meu fone de ouvido (A1) e para o chat de voz (B1). Note que este volume está em -15, pois assim a música fica com o volume diminuído.

### Retângulo Verde

Por fim, a entrada virtual auxiliar envia o som para o fone de ouvido (A1), permitindo que o que é dito no chat de voz seja ouvido.
