---
layout: post
title: "Bot do Telegram para controlar seu Home Assistant"
date: 2023-05-27 20:30:00 -0300
categories: [Automação Residencial, Home Assistant]
tags: telegram bot homeassistant hass assist
image: "/assets/img/HassTelegram.png"
---

> Conceitos necessários:
>
> [Como obter IDs no Telegram](https://blog.gabrf.com/posts/TelegramID/)
>
> [Como fazer bots para o Telegram](https://blog.gabrf.com/posts/HowToBot/).
>
> [Introdução ao Home Assistant](https://blog.gabrf.com/posts/HomeAssistant/).

# Introdução

Ter um bot vinculado ao seu Home Assistant pode ser bastante útil. Eis alguns casos:

* Fácil gestão de usuários fora do Home Assistant

O bot pode ser habilitado para aceitar comandos de um grupo do Telegram. Desta forma, a gestão de quem pode ligar ou desligar as luzes, por exemplo, passa a ser a simples gestão de quem está ou não no grupo.

* Controle mesmo quando o Home Assistant não é acessível pela internet

Caso o Home Assistant não esteja exposto na internet, o controle pode ser feito via bot. Ou seja, mesmo que haja um firewall de operadora bloqueando o acesso ao seu Home Assistant, sua residência pode ser gerida pelo Telegram em uma conversa. Não é necessário configurar redirecionamento de portas nem lidar com certificados https. Tudo funciona de maneira simples.

# Configuration.yaml

A configuração inicial do bot deve ser feita no arquivo `configuration.yaml`, como no exemplo abaixo:

```yaml
telegram_bot:
  - platform: polling
    api_key: 158700146:AAHOPReqqTR8V7FXysa8mJCbQACUWSTBog8
    parse_mode: html
    allowed_chat_ids:
      - 9083329 # Gabriel
      - 172293282 # Bia
```

* `platform`: Maneira que o bot irá receber as mensagens do Telegram. As opções são `polling` ou `webhook`;
* `api_key`: Token do seu bot;
* `parse_mode`: Tipo de formatação que seu bot usará. As opções são `html` ou `markdown`;
* `allowed_chat_ids`: Lista de IDs que podem usar o bot. Podem ser IDs de pessoas ou de grupos.

---

> De agora em diante, todas as configurações do bot no Home Assistant serão feitas nas <b>Automações</b>. Abra o Home Assistant e, no menu esquerdo, clique em `Configurações` e em `Automações & Cenas`.

# Comando /start

Um bot sempre é iniciado pelo comando `/start`. Por este motivo, recomendo que este comando esteja presente na configuração, retornando uma mensagem de início, uma introdução ao bot. Além de servir como primeiro contato, o comando `/start` pode ser uma maneira fácil de verificar se o bot está funcionando.

Clique em `Criar automação`, `Criar nova automação`. No canto superior direito, clique nos três pontinhos e, em seguida, em `Editar como YAML`.

Copie e cole o código:

{%raw%}
```yaml
alias: Telegram Comando start
description: ""
trigger:
  - platform: event
    event_type: telegram_command
    event_data:
      command: /start
condition: []
action:
  - service: telegram_bot.send_message
    data:
      message: |
        <b>Home Assistant bot</b>
        ↙️ Clique em <i>Menu</i> para ver os comandos.
      target: {{ trigger.event.data.chat_id }}
mode: single
```

* `alias`: Nome da automação;
* `description`: Descrição;
* `trigger`: Gatilho, o que faz a automação iniciar;
  * `platform`: Tipo de plataforma do gatilho;
  * `event_type`: Tipo de evento;
  * `event_data`: Dado do evento;
    * `command`: Comando;
* `condition`: Condição, o que impede ou não a automação;
  * Nenhuma condição definida;
* `action`: Ação, o que a automação de fato faz;
  * `service`: Tipo de serviço;
  * `data`: Dados usados na ação;
    * `message`: Mensagem enviada. `|` é usado para casos de múltiplas linhas;
    * `target`: Destinatário da mensagem. O valor `{{ trigger.event.data.chat_id }}` faz responder no mesmo chat em que o comando foi enviado;
* `mode`: Modo de execução. Ou seja, se a automação pode ser executada em múltiplas tarefas ou não.

**Pronto!** Seu bot já deve estar respondendo ao comando `/start`. Para criar mais comandos, duplique esta automação fazendo o ajuste que julgar necessário.

<img src="/assets/img/HassTelegramStart.png" alt="Comando `/start`." style="width:50%">
<br><small>Comando `/start`.</small>

# Comando /luzes e exibição de botões

Outro exemplo de comando que pode ser criado é o que exibe e permite controlar algumas luzes usando botões.

```yaml
alias: Telegram Comando Luzes
description: ""
trigger:
  - platform: event
    event_type: telegram_command
    event_data:
      command: /luzes
condition: []
action:
  - service: telegram_bot.send_message
    data:
      message: <b>Controle das Luzes</b>
      target: "{{ trigger.event.data.chat_id }}"
      inline_keyboard:
        - - - >-
              Quarto teto {{ states.switch.sonoff_1000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_1000000000
        - - - >-
              Esquerda {{ states.switch.sonoff_2000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_2000000000
          - - >-
              Direita {{ states.switch.sonoff_3000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_3000000000
(...)
        - - - >-
              Escritório {{ states.switch.sonoff_4000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_4000000000
        - - - Atualizar 🔄
            - /atualizar
mode: single

```

<img src="/assets/img/HassTelegramLuzes.png" alt="Comando `/luzes`." style="width:50%">
<br><small>Comando `/luzes`.</small>

## Disposição

* `inline_keyboard`: Inicia a criação de botões;
  * `- - -`: Indica nova linha;
  * `- -`: Indica um botão na mesma linha;

## Botões

Todo botão tem uma mensagem exibida e um comando a ser enviado:
```yaml
        - - - >-
              Quarto teto {{ states.switch.sonoff_1000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_1000000000
```

Ou seja, este botão começa uma nova linha, possui a mensagem `Quarto teto 💡` e o comando `sonoff_1000000000`.
* `{{ states.switch.sonoff_1000000000 }}` representa o estado do botão;
* `|replace("on", "💡")|replace("off", "⚫️")` faz o estado ser simbolizado por 💡 caso esteja ligado (*on*) ou ⚫️ caso esteja desligado (*off*).

# Comando do botão Atualizar

Diferentemente do comando de texto, o comando de um botão vem em um `callback`. Os botões do comando anterior exibem somente o estado de quando foram gerados. Para atualizar os estados sem que seja necessário repetir o comando `/luzes`, sugiro o uso do botão `Atualizar`. Crie uma nova automação com o seguinte conteúdo:

```yaml
alias: Telegram Callback Atualizar
description: ""
trigger:
  - platform: event
    event_type: telegram_callback
    event_data:
      command: /atualizar
condition: []
action:
  - service: telegram_bot.answer_callback_query
    data:
      callback_query_id: "{{ trigger.event.data.id }}"
      message: |
        Atualizado em {{ now().hour }}:{{ now().minute }}:{{ now().second }}
      show_alert: true
  - service: telegram_bot.edit_message
    data:
      message_id: "{{ trigger.event.data.message.message_id }}"
      chat_id: "{{ trigger.event.data.chat_id }}"
      message: |
        <b>{{ trigger.event.data.message.text }}</b>
      inline_keyboard:
        - - - >-
              Quarto teto {{ states.switch.sonoff_1000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_1000000000
(...)
        - - - Atualizar 🔄
            - /atualizar
mode: single

```

* `service`: Responder a chamada do botão. Valor: `telegram_bot.answer_callback_query`;
* `callback_query_id`: Responder para quem clicou no botão. Valor: `"{{ trigger.event.data.id }}"`;
* `show_alert`: Exibir *pop-up*. Valores possíveis: `true` ou `false`.

Ou seja, sempre que o botão `Atualizar` for apertado, um *pop-up* será exibido confirmando a atualização naquele horário e a mensagem será editada para exibir os valores atuais.

<img src="/assets/img/HassTelegramLuzesPopUp.png" alt="Pop-up do botão atualizar." style="width:50%">
<br><small>Pop-up do botão atualizar.</small>

# Comando de botão de luz

```yaml
alias: Telegram Callback
description: ""
trigger:
  - platform: event
    event_type: telegram_callback
condition: []
action:
  - service: switch.toggle
    data: {}
    target:
      entity_id: switch.{{ trigger.event.data.data }}
  - delay:
      hours: 0
      minutes: 0
      seconds: 0
      milliseconds: 100
  - service: telegram_bot.edit_message
    data:
      message_id: "{{ trigger.event.data.message.message_id }}"
      chat_id: "{{ trigger.event.data.chat_id }}"
      message: <b>{{ trigger.event.data.message.text }}</b>
      inline_keyboard:
        - - - >-
              Quarto teto {{ states.switch.sonoff_1000000000.state|replace("on",
              "💡")|replace("off", "⚫️") }}
            - sonoff_1000000000
(...)
        - - - Atualizar 🔄
            - /atualizar

```

O botão de luz inicia três ações em sequência:

1. O `switch`, cujo valor vem pelo comando do botão(`{{ trigger.event.data.data }}`), tem seu estado invertido (*toggle*);
2. Aguarda 100 milisegundos para que o `switch` tenha seu estado alterado;
3. Atualiza a mensagem, refazendo o seu conteúdo com os estados atualizados.

# Comando /camera

Este comando tira uma foto da câmera e a envia por Telegram. Muito útil para verificar rapidamente o que está sendo gravado.

```yaml
alias: Telegram Comando Camera
description: ""
trigger:
  - platform: event
    event_type: telegram_command
    event_data:
      command: /camera
condition: []
action:
  - service: camera.snapshot
    data:
      filename: /tmp/snapshot.png
    target:
      device_id: 123456789abcdefghijklmnopqrst
  - service: telegram_bot.send_document
    data:
      authentication: digest
      file: /tmp/snapshot.png
      target: "{{ trigger.event.data.chat_id }}"
mode: single

```

As duas ações desencadeadas pelo comando são:

1. Tirar uma foto (`camera_snapshot`) usando a câmera definida em `device_id` e a armazena no caminho definido em `filename`;
2. Envia a foto em formato de `documento`. A vantagem de enviar a foto como documento é que mantém a qualidade original. Para enviar a foto com compressão, tornando o envio mais rápido mas com perda de qualidade, substitua `send_document` por `send_photo`. 

## Falha "*no access to path*"

Caso a pasta não esteja acessível à ação de tirar a foto, um erro "*no access to path*" será exibido no log. Para corrigir, abra o `configuration.yaml` e adicione em `homeassistant`:

```yaml
homeassistant:
(...)
  allowlist_external_dirs:
    - "/tmp"

```

# Adicionar itens à lista de compras

Para adicionar itens à Lista de Compras, [integração disponível aqui](https://www.home-assistant.io/integrations/shopping_list/), basta enviar uma mensagem para o bot contendo o termo `comprar`.

```yaml
alias: Telegram Lista de Compras
description: ""
trigger:
  - platform: event
    event_type: telegram_text
condition:
  - condition: template
    value_template: "{{ 'comprar' in trigger.event.data.text|lower }}"
action:
  - service: shopping_list.add_item
    data:
      name: "{{ trigger.event.data.text|title|replace('Comprar', '') }}"
  - service: shopping_list.sort
    data:
      reverse: false
  - service: telegram_bot.send_message
    data:
      message: |
        <code>{{ trigger.event.data.text }}</code>
        Salvo na Lista de Compras.
      target: "{{ trigger.event.data.chat_id }}"
mode: single

```

* `condition`: Condição em que a ação será executada. Neste caso, ela somente será executada na presença do termo `comprar`;
  * `|lower` considera a mensagem toda em minúsculo, facilitando a verificação da condição.

As três ações da automação são:

1. Adicionar o item à lista de compras;
  * `|title`: Escreve o item em formato de título, ou seja, com as iniciais em maiúsculo;
  * `|replace('Comprar', '')`: Remove o termo "Comprar" presente na mensagem;
2. Ordena a lista de compras;
  * Por não haver verificação de itens duplicados, faz-se necessária a ordenação alfabética;
3. Envia uma mensagem confirmando que o item foi adicionado à lista de compras.

<img src="/assets/img/HassTelegramLuzesAddToList.png" alt="Adicionando item à lista de compras." style="width:50%">
<br><small>Adicionando item à lista de compras.</small>

<img src="/assets/img/HassTelegramShoppingList.png" alt="Item adicionado." style="width:50%">
<br><small>Item adicionado.</small>

# Comandos usando Assist

Outra maneira de controlar o Home Assistant é usando o [Assist](https://www.home-assistant.io/docs/assist/), usando comandos em forma de texto.

```yaml
alias: Telegram Assist
description: ""
trigger:
  - platform: event
    event_type: telegram_text
condition:
  - condition: template
    value_template: "{{ 'comprar' not in trigger.event.data.text|lower }}"
action:
  - service: conversation.process
    data:
      text: "{{ trigger.event.data.text }}"
      language: pt-br
  - service: telegram_bot.send_message
    data:
      message: |
        <code>{{ trigger.event.data.text }}</code>
        Comando enviado.
      target: "{{ trigger.event.data.chat_id }}"
      parse_mode: html
mode: single

```

Esta automação somente será executada caso o termo `comprar` não esteja presente na mensagem. Satisfeita a condição, é iniciado o serviço `conversation.process`.

```yaml
  - service: conversation.process
    data:
      text: "{{ trigger.event.data.text }}"
      language: pt-br
```

Este serviço irá processar a mensagem recebida e executar o comando. Definir o idioma não é obrigatório, mas torna a ação muito mais rápida, portanto, é recomendável ter o idioma ajustado.

Por fim, a ação notifica o usuário que foi executada.

<img src="/assets/img/HassTelegramAssist.png" alt="Assist em funcionamento." style="width:50%">
<br><small>Assist em funcionamento.</small>

{%endraw%}

# Menu do bot

O menu do bot não é obrigatório, mas é recomendado por facilitar a localização dos comandos disponíveis. A maneira mais fácil de criar o menu é falando com o [@BotFather](https://t.me/BotFather). Envie o comando `/mybots` e selecione o bot que deseja ajustar na lista. Clique em `Edit Bot` e, em seguida, em `Edit Commands`. Envie os comandos no formato `comando - Descrição`.

<img src="/assets/img/HassTelegramBotFather.png" alt="Ajuste de comandos no BotFather." style="width:50%">
<br><small>Ajuste de comandos no BotFather.</small>

<img src="/assets/img/HassTelegramCommands.png" alt="Bot com o menu de comandos definido." style="width:50%">
<br><small>Bot com o menu de comandos definido.</small>
