---
layout: post
title: "Alarme inteligente - Como nunca perder compromissos usando o Home Assistant"
date: 2026-02-08 21:00:00 -0300
categories: [Automação Residencial, Home Assistant]
tags: homeassistant alarme notificação
image: "/assets/img/CalendarAlarm.png"
---

# Alarme Inteligente

> Será que eu ajustei o alarme de amanhã?

Cansado de ser acordado em feriados por ter esquecido de desligar o alarme, busco há muito tempo uma solução para deixar tudo mais inteligente. Os requisitos eram: 
1. Somente tocar nos dias certos e de forma automatizada;
2. Preferencialmente tocar mesmo com o celular no modo silencioso;
3. Se possível, tocar no relógio inteligente, sem incomodar outras pessoas. 

## Quando o alarme vai funcionar?

O passo inicial foi adicionar o calendário de compromissos ao Home Assistant. 

1. Exportar o arquivo `.ics` da agenda de compromissos;
2. Abrir o Home Assistant, ir em `Calendário`, `Meus Calendários`, `Criar calendário`;
3. Selecionar a opção `Upload an iCalendar file (.ics)` e fazer o envio do arquivo.

## Home Assistant e Alertas Críticos

Com as informações de compromissos já disponíveis, o Home Assistant poderia fazer uma infinidade de coisas como alarme: Controlar alguma luz, tocar uma música, ligar a tv, abrir as cortinas, tocar a campainha. Minha opção foi o alerta crítico. 

Alerta crítico é uma notificação disponível nos celulares em que a notificação funciona mesmo quando o celular está no modo silencioso. É possível definir o volume do toque, o título do alerta e sua mensagem. Além disso, é possível definir qual janela do Home Assistant será aberta ao clicar na notificação.

## Relógios Inteligentes e Notificações Sem Som
 
O último critério a ser atendido é o de não incomodar outras pessoas. O _Wife Acceptance Factor_ é o maior obstáculo de qualquer pessoa que mexe com automação residencial. Garantir que a esposa não será acordada sem necessidade é primordial.

Como usamos relógios inteligentes, uma notificação no telefone automaticamente vira uma notificação no relógio. Deixando o volume em zero, o relógio irá apenas vibrar, garantindo que somente quem precisa será acordado. No caso abaixo, a notificação é repetida a cada 2 segundos até que o telefone seja acionado e o botão de desativar o alarme clicado.

## Arquivo YAML da automação

```yaml
alias: Alarme Gabriel
description: "Alarme crítico sem volume baseado no calendário"
triggers:
  - trigger: calendar
    entity_id: calendar.gabriel
    event: start
    offset: "-2:30:0"
conditions:
  - condition: time
    before: "12:00:00"
actions:
  - action: input_boolean.turn_on
    target:
      entity_id: input_boolean.alarme_gabriel
    data: {}
  - repeat:
      until:
        - condition: state
          entity_id: input_boolean.alarme_gabriel
          state: "off"
      sequence:
        - data:
            title: "Alarme ⏰ "
            data:
              url: /dashboard-alarme/0
              tag: alerta
              push:
                badge: 5
                sound:
                  critical: 1
                  name: default
                  volume: 0
            message: "Clique para desativar "
          action: notify.mobile_app_gabriel
        - delay:
            hours: 0
            minutes: 0
            seconds: 2
            milliseconds: 0
mode: single
```

* `alias`: Nome da automação;
* `description`: Descrição;
* `triggers.trigger`: Tipo de gatilho da automação. `calendar` é o valor para se basear no calendário;
* `triggers.entity_id`: ID da entidade do calendário;
* `triggers.event`: Critério de início da automação. `start` é para o início do evento;
* `triggers.offset`: Valor de desvio. `-2:30` faz o alarme tocar 2h30 antes do início do evento;
* `conditions.condition`: Verifica uma condição após o acionamento do gatilho. `time` verifica o horário;
* `conditions.before`: A condição a ser atendida é um horário antes do valor definido. No caso, antes do meio dia, ou seja, somente pela manhã.

### `Actions`

As configurações das ações dependem do que será acionado em cada caso. No exemplo, os passos são:
1. Garante que o botão de `Alarme` esteja ligado;
2. Inicia o loop;
  - Define que a ação irá repetir enquanto o botão de `Alarme` não seja desligado;
  - Envia um alerta crítico e sem som que, ao ser clicado, abre a janela `/dashboard-alarme/0`;
  - Aguarda 2 segundos antes de repetir.

Ou seja, o alarme somente será encerrado após a pessoa desligar o botão de `Alarme`, que pode ser facilmente acessado clicando-se na notificação.
