---
layout: post
title: "Não perturbe. Em reunião."
date: 2023-11-21 18:30:00 -0300
categories: [Automação Residencial, MQTT]
tags: automação mqtt python hass homeassistant
image: "/assets/img/DoNotDisturbREC.png"
---
{%raw%}
> "Estou em reunião. Falo com você em seguida".

Este pequeno script Python monitora o estado de sua *webcam* e informa, em tempo real, se ela está sendo usada. Por meio de um tópico [*MQTT*](https://blog.gabrf.com/posts/MQTT/), todos os dispositivos da rede podem consultar o uso da câmera, podendo iniciar, por exemplo, uma automação que acende uma luz. Solução importantíssima para os profissionais que atuam remotamente e, em muitos casos, não podem ser incomodados.

Apesar de ser escrito em Python, a solução aborda alguns comandos e conceitos do Linux, tais como:

## lsmod

Lista os módulos do *kernel*[^kernel] que estão carregados na memória. Ao ser executado, sua saída será parecida com:

```bash
$ lsmod

Module                  Size  Used by
nfnetlink              20480  0
hidp                   40960  1
tls                   151552  4
nvidia_uvm           1802240  8
uvcvideo              135168  0
```

Repare que a primeira coluna contém o nome do módulo e a última contém os módulos que estão utilizando o recurso.

## uvcvideo

*USB Video Class* - Classe de vídeo USB. É o módulo que descreve dispositivos de vídeo, como *webcams*. Ou seja, ao ser executado o comando [`lsmod`](#lsmod), é possível ver quais processos estão utilizando o recurso de vídeo.

### *Webcam*  desligada

```bash
$ lsmod | grep uvcvideo

uvcvideo              135168  0
```

### *Webcam* em uso

```bash
$ lsmod | grep uvcvideo

uvcvideo              135168  1
```

# WebcamStatus2MQTT

## Script Python

O único requisito deste script é o pacote [Paho-MQTT](https://pypi.org/project/paho-mqtt/), que pode ser instalado usando-se `pip install paho-mqtt`

```python
import paho.mqtt.client as mqtt
import os
import subprocess

MQTTSERVER = ''            # Servidor MQTT, também chamado de broker
MQTTUSER = ''              # Usuário MQTT
MQTTPASS = ''              # Senha do usuário MQTT
MQTTTOPIC = 'Webcam'       # Tópico a ser usado
CLIENTID = 'WebcamStatus'  # Identificador único do cliente MQTT

def run_lsmod():           # Função que lista os módulos do kernel
    lsmod = subprocess.run(['lsmod'], capture_output=True, text=True)
    lines = lsmod.stdout.splitlines()

    for line in lines:
        if 'uvcvideo' in line:
            return line

def post_mqtt(value):      # Função que faz a publicação no tópico MQTT
    mqttclient = mqtt.Client(client_id=CLIENTID)
    mqttclient.username_pw_set(MQTTUSER, password=MQTTPASS)
    mqttclient.connect(MQTTSERVER)
    mqttclient.publish(f'{MQTTTOPIC}/state', value)

if __name__ == "__main__":
    status = run_lsmod()
    post_mqtt(status.split()[-1])
```

* `run_lsmod`: Função que executa o comando `lsmod` e captura seu resultado. Em seguida, busca pela *string* "*uvcvideo*" e retorna a primeira linha em que ela é encontrada;
* `post_mqtt`: Recebe o último dígito do resultado da função `lsmod` e o publica em um tópico do *broker* MQTT.

## Cronjob

O arquivo Python é executado minuto a minuto por meio de um *cronjob*[^cronjob] na máquina em que ocorrem as reuniões.

```shell
* * * * * cd /home/gabriel/WebcamStatus2MQTT; /usr/bin/python3 WebcamStatus2MQTT.py
```

# [Home Assistant](https://blog.gabrf.com/posts/HomeAssistant/)

## Sensor

Para receber os dados via MQTT, abra o arquivo `configuration.yaml` e adicione:

```yaml
mqtt:
  sensor:
    - name: StatusWebcam
      state_topic: "Webcam/state"
      value_template: "{{ float(value) }}"
      unique_id: "mqtt_sensor_webcam"
```

Por fim, para controlar um dispositivo ligando-o ou desligando-o, conforme o estado da webcam, são criadas duas automações:

## Automação Webcam em funcionamento

```yaml
alias: Webcam Ligada
description: ""
trigger:
  - platform: state
    entity_id:
      - sensor.statuswebcam
    from: "0.0"
    to: "1.0"
condition: []
action:
  - service: switch.turn_on
    data: {}
    target:
      entity_id:
        - switch.sonoff_100000000
mode: single
```

Isto é, se o valor de `sensor.statuswebcam` variar de `0.0` para `1.0`, então o dispositivo `switch.sonoff_100000000` é ligado, acendendo uma lâmpada.

## Automação Webcam desligada

```yaml
alias: Webcam Desligada
description: ""
trigger:
  - platform: state
    entity_id:
      - sensor.statuswebcam
    from: "1.0"
    to: "0.0"
condition: []
action:
  - service: switch.turn_off
    data: {}
    target:
      entity_id:
        - switch.sonoff_100000000
mode: single
```

Ou seja, quando o valor de `sensor.statuswebcam` sair de `1.0` para `0.0`, quando a câmera é desligada, a luz controlada pelo `switch.sonoff_100000000` é desligada.

## Template de Sensor

Um *template* no Home Assistant permite criar entidades com base em outros valores. No exemplo abaixo é criado um *sensor* que varia o ícone, o estado e o nome com base no [`sensor.statuswebcam`](#sensor).

```yaml
template:
  - sensor:
    - unique_id: template_sensor_em_reuniao
      icon: >
        {% if states('sensor.statuswebcam')|float == 0 %} mdi:account
        {% elif states('sensor.statuswebcam')|float == 1 %} mdi:account-group
        {% endif %}
      state: >
        {% if states('sensor.statuswebcam')|float == 0 %} off
        {% elif states('sensor.statuswebcam')|float == 1 %} on
        {% endif %}
      name: >
        {% if states('sensor.statuswebcam')|float == 0 %} Disponível
        {% elif states('sensor.statuswebcam')|float == 1 %} Em reunião
        {% endif %}
```

* `unique_id`: Id único da entidade que está sendo criada;
* `icon`: Ajustes de ícone;
* `state`: Ajustes de estado;
* `name`: Ajustes de nome.

Todos os ajustes baseiam-se nos mesmos casos:
* Se o `sensor.statuswebcam` for igual a `0`, então a *webcam*  está desligada e, portanto, não há reunião;
* Caso o valor do `sensor.statuswebcam` seja igual a `1`, então uma reunião está acontecendo.

<img src="/assets/img/DoNotDisturbTemplateSensor.jpg" alt="Exemplo de sensor criado por template." style="width:50%">
<br><small>Exemplo de sensor criado por template. Sem reunião (acima) e com reunião acontecendo.</small>

---

[^kernel]: É, basicamente, a parte do sistema operacional responsável pela comunicação entre o *software* e o *hardware* dos computadores.

[^cronjob]: Uma tarefa executada periodicamente, seguindo um cronograma.
{%endraw%}
