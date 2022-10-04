---
layout: post
title: "Automação Residencial: Interruptor de cenas"
date: 2022-10-04 18:040:00 -0300
categories: [Automação Residencial, Interruptor de Cenas]
tags: automação cenas sonoff smate homeassistant hass
image: "/assets/img/sonoff_dualr3.jpg"
---

> Resolvendo todos os problemas que você não sabia que tinha.

Um **interruptor comum** é aquele que controla somente um circuito. A ligação entre o interruptor e o dispositivo controlado é um fio, limitando completamente sua ação para um fim específico. Não é possível alterar o que é controlado por um interruptor comum sem que se desligue o quadro elétrico, abra as caixinhas dos interruptores e sejam feitas mudanças na fiação.

Já um **interruptor de cenas** é um interruptor que ao ser apertado pode executar uma ação qualquer. Não existe ligação física direta entre o interruptor de cenas e os objetos. Tudo é definido por software. Ou seja, é possível configurar um interruptor de cenas para apagar somente uma lâmpara ou apagar todas as lâmpadas de uma vez, sem que seja necessária nenhuma intervenção na instalação. Tudo é feito via software.

### Sonoff S-Mate

<center>
<img src="/assets/img/sonoff_smate.jpg" alt="Sonoff S-Mate" style="width:50%; border-radius:5%"> 
<br><small>Sonoff S-Mate</small>
</center>

O S-Mate é um dispositivo que transforma um interruptor comum em um **interruptor de cenas**, podendo ser instalado em caixas 4x2. Ele não exige neutro e pode ser colocado em locais em que foi feita uma instalação padrão para iluminação. Ele pode receber até três interruptores diferentes. Exige a existência de um [Sonoff MiniR3](#sonoff-minir3) ou de um [Sonoff M5](#sonoff-m5) a no máximo 40 metros de distância para que funcione. Havendo este pareamento com o MiniR3 ou com o M5, torna-se então capaz de controlar qualquer outro dispositivo.

Seu único ponto negativo é a alimentação por bateria CR2032. Ou seja, mesmo com uma expectativa de duração de dois anos, não é possível instalá-lo e esquecer que existe. Mais cedo ou mais tarde será necessário fazer uma pequena manutenção, abrindo-o e trocando a bateria. Por isso, analise muito bem onde irá instalá-lo.

De todos os dispositivos deste post, o S-Mate é o que mais me chama a atenção por permitir que um interruptor comum seja transformado em um interruptor de cenas. Um interruptor que ligava uma lâmpada pode passar a desligar cinco lâmpadas diferentes, ligar a tv e o ar condicionado. Uma campainha pode tocar uma música[^homeassistant]. As possibilidades passam a ser infinitas.

#### Instalação

Um ponto que me chamou a atenção no Sonoff S-Mate é a sua instalação. Ele possui os pontos `L`, `L1`, `L2` e `L3`, que servem para manter em funcionamento as ligações já existentes nas caixas 4x2. Porém, **usá-las é totalmente opcional** uma vez que elas não participam do funcionamento do dispositivo. Na verdade, estes quatro pontos são fisicamente unidos internamente. Qualquer dispositivo ligado após um destes pontos receberá energia sempre. Por este motivo o fabricante indica a instalação do [Sonoff MiniR3](#sonoff-minir3) na sequência do S-Mate. O MiniR3 instalado desta forma nunca é desligado e ambos os dispositivos se encaixam perfeitamente em ligações padrões de luzes.

A parte importante na ligação do S-Mate acontece nos pontos `S`, `S1`, `S2` e `S3`. Estes sim são os pontos que servem de gatilho para as cenas. O ponto `S` é o "comum" e sempre que é fechado o circuito entre ele e qualquer um dos outros pontos `S` é enviado um sinal para o [Sonoff MiniR3](#sonoff-minir3) ou para o [Sonoff M5](#sonoff-m5), o que estiver vinculado.

<center>
<img src="/assets/img/sonoff_smate_wiring.jpg" alt="Esquema de ligação do Sonoff S-Mate" style="width:70%; border-radius:5%"> 
<br><small>Esquema de ligação do Sonoff S-Mate</small>
</center>

##### Por que isso é bom?

Porque permite que facilmente seja criado um interruptor novo sem que chegue fio algum ao local. É possível, por exemplo, ligar os pontos `S` e `S1` em uma campainha sem que seja necessário ter ponto de energia elétrica. Desta forma, ao ser tocada a campainha, uma cena será iniciada, que pode desde acender uma luz até, por exemplo, enviar uma mensagem para um celular[^homeassistant].

Um interruptor de cenas pode também ter seu comportamento variando conforme o horário. É possível que o interruptor ao lado da cama acenda a luz de leitura caso acionado até as 21:00. Após as 21:00, se clicado, poderia enviar um comando desligando todas as luzes da casa, deixando somente o jardim aceso. Ao acordar, caso seja acionado até as 10:00 e for um dia de semana, desligaria o ar condicionado.

[^homeassistant]: O controle de outros dispositivos que não são Sonoff exige o uso de soluções de terceiros, como, por exemplo, o [Home Assistant](/posts/HomeAssistant/).

### Sonoff MiniR3

<center>
<img src="/assets/img/sonoff_minir3.jpg" alt="Sonoff Mini R3" style="width:50%; border-radius:5%">
<br><small>Sonoff Mini R3</small>
</center>

Quase um Sonoff Basic, o <u>Sonoff Mini R3 não é um interruptor</u>, mas pode receber o sinal do [Sonoff R5](#sonoff-r5) e do [Sonoff S-Mate](#sonoff-s-mate). É um dispositivo em que entram dois fios, fase e neutro, e saem também dois fios, fase e neutro. Ou seja, para fazer a instalação em uma luz já existente, basta colocar o dispositivo ali, onde a lâmpada tem seus fios conectados. O Mini R3 será um intermediário entre a lâmpada e a fiação do local. Não há espaço para a ligação do interruptor nem mais nada.

Seu grande trunfo é permitir o uso do [Sonoff S-Mate](#sonoff-s-mate) uma vez que ambos têm a instalação simplificada. A desvantagem é o aumento do custo, pois é necessário comprar dois dispositivos em vez de um só.

Porém, como um único Mini R3 é capaz de se comunicar com até oito S-Mates, não é necessário colocar o Mini R3 em todos os circuitos a serem controlados. Qualquer outro Sonoff atenderia, mesmo o Basic.

### Sonoff DualR3

<center>
<img src="/assets/img/sonoff_dualr3.jpg" alt="Sonoff Dual R3" style="width:50%; border-radius:5%">
<br><small>Sonoff Dual R3</small>
</center>

Este modelo parece o [Sonoff MiniR3](#sonoff-minir3), mas, na verdade, é a evolução do [Sonoff Mini R2](/posts/AutomationSwitch/#internos). Pode controlar até dois circuitos separadamente, é capaz de controlar motores[^dualr3motores] e é compatível com interruptores *three-way*. Pode ser encontrado em dois modelos:

[^dualr3motores]: Consulte informações no [site do fabricante](https://itead.cc/product/sonoff-dualr3/).

#### Dual R3 Lite

Versão básica. Possui as mesmas funcionalidades do [Sonoff Mini R2](/posts/AutomationSwitch/#internos), tendo como novidade o controle de motores[^dualr3motores] e entrada máxima de 15A no total.

#### Dual R3

Apesar de ter a mesma aparência da versão Lite, possui como diferenciais a capacidade de calibração para motores e também a de **medição do consumo de energia elétrica**, permitindo a visualização de histórico e de dados em tempo real.

#### Limitação

Não serve como dispositivo que recebe o sinal do [Sonoff S-Mate](#sonoff-s-mate) nem do [Sonoff R5](#sonoff-r5). Isto significa que um S-Mate ou um R5 é sim capaz de interagir com o DualR3, desde que exista um [Sonoff M5](#sonoff-m5) ou um [Sonoff MiniR3](#sonoff-minir3) como intermediários.

### Sonoff M5

<center>
<img src="/assets/img/sonoff_m5.webp" alt="Sonoff M5" style="width:50%; border-radius:5%">
<br><small>Sonoff M5 (modelo 120)</small>
</center>

O Sonoff M5 é um **interruptor comum**, porém, capaz de se comunicar com o [Sonoff S-Mate](#sonoff-s-mate) e com o [Sonoff R5](#sonoff-r5). Infelizmente sua estética não é a mesma do [Sonoff R5](#sonoff-r5), deixando um ambiente sem padrão caso sejam usados em conjunto. Tem exatamente o mesmo funcionamento do [MiniR3](#sonoff-minir3), porém em formato de interruptor. Exige a existência de um fio neutro para funcionar. É a evolução da linha TX, com visual renovado e a capacidade de se comunicar com o [S-Mate](#sonoff-s-mate) e com o [R5](#sonoff-r5).

O modelo compatível com as caixas 4x2 brasileiras é o 120, que possui opções com uma, duas ou três teclas.

<center>
<img src="/assets/img/sonoff_m5_models.png" alt="Opções de Sonoff M5 120" style="width:70%; border-radius:5%">
<br><small>Opções de Sonoff M5 120</small>
</center>

### Sonoff R5

<center>
<img src="/assets/img/sonoff_r5.jpg" alt="Sonoff R5" style="width:50%; border-radius:5%">
<br><small>Sonoff R5</small>
</center>

O Sonoff R5 é um **interruptor de cenas** sem fio. Ele é acompanhado uma base magnética que pode ser fixada na parede, cujo uso é opcional. O dispositivo é alimentado por duas baterias CR2032 e se comunica com o [Sonoff M5](#sonoff-m5) ou com o [Sonoff MiniR3](#sonoff-minir3) em 433Mhz. Seu grande trunfo em relação ao uso do [S-Mate](#sonoff-s-mate) é o de permitir além do clique simples, a configuração de duplo clique e clique longo. Ou seja, uma mesma tecla pode ter três funções diferentes. Possuindo seis teclas, cada uma com três configurações, **permite o controle de até 18 cenas**.
