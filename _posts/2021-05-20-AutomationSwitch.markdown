---
layout: post
title: "Automação residencial: Tomadas e Interruptores"
date: 2021-05-20 14:00:00 -0300
categories: [Automação Residencial, Tomadas e Interruptores]
tags: automação interruptores tomadas
image: "/assets/img/sonoffapp.png"
---

> Jarvis, apague as luzes.

O controle de luzes e de tomadas é provavelmente a parte da automação residencial com mais opções disponíveis. Nesse post tentarei explicar os pontos que considero importantes para que seja feita a melhor escolha para a sua realidade.

## Tomadas

Começando pelo mais fácil, a tomada costuma ser resolvida simplesmente acrescentando um dispositivo entre as conexões. O único ponto importante a ser observado é a corrente máxima suportada pela tomada inteligente, que costuma ser 10A (2200W). Essa corrente costuma ser suficiente para a maior parte dos dispositivos domésticos, mas dificilmente será possível utilizar em uma máquina de lavar roupas, em um chuveiro ou um forno elétrico.

Antes de comprar, verifique a compatibilidade do dispositivo. Prefira os dispositivos que integram com os assistentes de voz do Google e da Amazon (Alexa), pois estes potencialmente conversam com outros dispositivos que você possuir, permitindo integrações muito interessantes, como apagar as luzes e as tomadas com um comando só ou até criar um cenário em que alguns dispositivos são ligados e outros desligados, permitindo uma melhor experiência ao assistir TV, por exemplo.

<center>
<img src="/assets/img/sonoff_tomada.png" alt="Sonoff Smartplug no padrão brasileiro." style="width:40%"> 
<br><small>Sonoff Smartplug no padrão brasileiro.</small>
</center>

## Interruptores

Separados em 3 categorias, os interruptores são as partes que têm o potencial para serem as mais trabalhosas. A idéia da categorização é explicar separadamente as vantagens e desvantagens de cada abordagem. `Externos` são os interruptores que ficam na parede mesmo, como interruptores que estamos acostumados. `Remotos` são aqueles que funcionam como controle remoto, que não ficam fixos na parede. `Internos` são aqueles que ficam escondidos, dentro da parede ou atrás da base da lâmpada. 

### Externos

Os interruptores externos entram no lugar do interruptor comum, sendo instalados nas caixas 4x4 ou 4x2. A única mudança de uma instalação comum é que os interruptores inteligentes necessitam de um **Neutro** chegando até eles para que funcionem. Ou seja, há a necessidade de se ter um fio a mais, além dos comumente encontrados nos interruptores já instalados. Nestes casos, não costuma ser complicado puxar o **Neutro** de uma tomada próxima ou até mesmo da lâmpada que será controlada. 

Raramente estes modelos suportam o funcionamento em paralelo com outros interruptores, nas ligações chamadas *three way*.

<center>
<img src="/assets/img/sonoff_external.png" alt="Sonoff Smartplug no padrão brasileiro." style="width:40%"> 
<br><small>Sonoff Smartplug no padrão brasileiro.</small>
</center>

### Remotos

Mais versáteis, os interruptores remotos funcionam por sinais de rádio, emitindo sinais em 433MHz, Zigbee ou até Bluetooth. Não necessitam ficar em local fixo nem de alimentação externa (tomada). Porém, é necessário que exista um dispositivo capaz de receber e interpretar o sinal de rádio, podendo ser a própria lâmpada, um interruptor (interno ou externo) ou até mesmo um dispositivo específico para isto, os chamados *bridges*.

<center>
<img src="/assets/img/sonoff_remote.png" alt="Sonoff Smartplug no padrão brasileiro." style="width:40%"> 
<br><small>Sonoff Controle Remoto 433MHz com suporte de parede.</small>
</center>

### Internos

Interruptores internos são bons por não afetarem a estética, pois ficam ocultos. Ou seja, são ligados nos interruptores já existentes no local. Assim como os externos, também necessitam de um **Neutro** para que funcionem. Há modelos, como o da imagem, que cabem em caixas 4x2, ocupando, dentro da caixa, o espaço que seria de um interruptor comum. Alternativamente, esses interruptores podem ser instalados atrás do suporte da lâmpada, o que costuma ser mais fácil, pois ali já existem todos os fios necessários para o funcionamento do dispositivo. Outro detalhe importante é que há interruptores internos (como o da foto) que são compatíveis com ligações *three way*.

Em resumo, interruptores internos são os que não causam mudança alguma na estética do local, porém, são os que apresentam maior desafio técnico para ser instalado. O ideal é que sejam instalados na fase de obra, pois tudo já fica pronto e oculto.

<center>
<img src="/assets/img/sonoff_internal.png" alt="Sonoff Smartplug no padrão brasileiro." style="width:40%"> 
<br><small>Sonoff Mini.</small>
</center>

## Conclusão e Recomendação

Pessoalmente gosto muito da marca [Sonoff](https://www.itead.cc/). São dispositivos muito bem construídos e que são compatíveis com inúmeras soluções disponíveis no mercado. Já de fábrica são compatíveis com assistentes de voz (Google e Alexa) e com aplicativos de celular, não ficando restrito ao aplicativo do próprio fabricante. Caso seja do interesse, é possível também alterar o *firmware*, eliminando assim a comunicação do fabricante com o dispositivo.

A marca oferece um amplo espectro de dispositivos. Dentre eles, destaco:

`Sonoff Mini`: Interruptor interno. Pequeno, barato e compatível com ligações *three-way*. Wi-fi.

`Sonoff D1`: Dimmer para lâmpadas. Também interno. Não cabe em caixas 4x2 nem é compatível com *three-way*. Wi-fi.

`Sonoff RM433`: Controle 433MHz para envio de comandos. 433MHz.

`Sonoff RF Bridge 433MHz`: *Bridge* necessária para receber os comandos do controle remoto e enviá-los para os demais dispositivos. 433MHz e Wi-fi.

`Sonoff TX`: Interruptor externo, sensível ao toque, para controle de lâmpadas. Wi-fi.

`Sonoff Smartplug S26`: Controle de tomadas até 10A. Wi-fi.
