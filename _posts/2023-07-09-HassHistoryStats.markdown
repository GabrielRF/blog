---
layout: post
title: "Contagem de tempo de uso de equipamentos usando Home Assistant"
date: 2023-07-09 14:00:00 -0300
categories: [Automação Residencial, Home Assistant]
tags: hass homeassistant historystats how-to automação
image: "/assets/img/HassHistoryStatsRoomba.png"
---

# Sensor

A contagem de tempo no Home Assistant é feita usando-se um sensor do tipo [*History Stats*](https://www.home-assistant.io/integrations/history_stats/) e de um [*Utility Meter*](https://www.home-assistant.io/integrations/utility_meter/), ambos definidos no arquivo `configuration.yaml`. O primeiro tem a função de fazer a contagem enquanto o segundo é responsável pelo acúmulo dos dados das medições.

## *History Stats*

Vá até o arquivo `configuration.yaml` e crie um novo sensor, de plataforma `history_stats`, seguindo o exemplo:

{%raw%}
```yaml
sensor:
  - platform: history_stats
    name: Tempo Roomba
    entity_id: vacuum.roomba
    type: time
    state: 'cleaning'
    start: '{{as_timestamp(now().replace(hour=0).replace(minute=0).replace(second=0))}}'
    end: '{{ now() }}'
```

* `name`: Nome que será exibido na interface do Home Assistant;
* `entity_id`: Entidade a ser contada;
* `type`: Tipo do sensor;
  * Opções: `time`, `ratio` e `count`;
* `state`: Estado a ser contado;
  * Exemplo: `on`, que seria o estado de ligado;
* `start`: Quando começar a contagem;
* `end`: Quando encerrar a contagem.

Normalmente é necessário personalizar apenas os valores dos campos `name` e `entity_id`.

> Para consultar a lista de entidades de seu Home Assistant, navegue até **Configurações**. Em seguida, clique em **Dispositivos & Serviços** e em **Registro de Entidades**.

# *Utility Meter*

Ainda no `configuration.yaml`, crie os `utility meter` que julgar pertinente. Exemplos:

```yaml
utility_meter:
  tempo_roomba_diario:
    source: sensor.tempo_roomba
    cycle: daily
  tempo_roomba_semanal:
    source: sensor.tempo_roomba
    cron: 0 0 * * 0
  tempo_roomba_mensal:
    source: sensor.tempo_roomba
    cycle: monthly
```

A estrutura normalmente é um nome seguido de duas opções:

* `source`: Origem do dado, `sensor.` seguido do nome do sensor criado em `history_stats`
  * Sempre em letras minúsculas e com os espaços substituídos por `_`;
* `cycle`: Período de validade da contagem;
  * Opções: `quarter-hourly`, `hourly`, `daily`, `weekly`, `monthly`, `bimonthly`, `quarterly` e `yearly`.

Por padrão, as medições no Home Assistant são iniciadas em 0 minutos, 0 horas, segunda-feira, dia 1 e em Janeiro. Ou seja, no caso do contador semanal, a semana começa na **segunda-feira**. Para que a contagem semanal inicie-se no domingo é usada uma regra de `cron`, descrita abaixo:

* `cron: 0 0 * * 0`: 0 minutos, 0 horas, todos os dias, todos os meses, aos domingos.

# Template

Finalmente, para se ter ícones alterados conforme o tempo de uso ou outra condição, usa-se o [*Template*](https://www.home-assistant.io/integrations/template/). Tal personalização permite a consulta rápida de acontecimentos diretamente na interface. Abaixo, um exemplo para saber se o aspirador de pó já passou hoje ou não.

<img src="/assets/img/HassHistoryStatsRoombaIcon.gif" alt="Tela de resumo do robô aspirador de pó." style="width:50%">
<br><small>Tela de resumo do robô aspirador de pó.</small>

No `configuration.yaml` é definido:

```yaml
template:
  - sensor:
    - name: "Roomba limpou hoje"
      state: >
        {{ states('sensor.tempo_roomba_diario') }}
      icon: >
        {% if states('binary_sensor.roomba_bin_full') == "on" %} mdi:robot-dead-outline
        {% elif states('sensor.tempo_roomba_diario')|float > 0 %} mdi:robot-happy-outline
        {% else %} mdi:robot-angry-outline
        {% endif %}
```

* `name`: Nome da nova entidade cujo ícone irá variar de acordo com as regras;
* `state`: Define um modelo para obter o estado do sensor;
* `icon`: Define o ícone;

No exemplo, as regras de ícone são:

1. Se o estado de `binary_sensor.roomba_bin_full` é `ligado`, ou seja, lixeira cheia, então o ícone é definido como `mdi:robot-dead-outline`;
2. Se o estado de `sensor.tempo_roomba_diario` for maior que zero, isto é, limpeza de hoje já iniciada, então `mdi:robot-happy-outline`;
3. Caso contrário, `mdi:robot-angry-outline`

{%endraw%}
