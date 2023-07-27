---
layout: post
title: "Transcrição automática de mensagens de voz no Telegram"
date: 2023-07-26 23:00:00 -0300
categories: [Telegram, Automação]
tags: telegram automação whisper openai pyrogram
image: "/assets/img/WhisperPyrogram.jpg"
---

{%raw%}
# [Pyrogram](https://docs.pyrogram.org/)

Esta é uma biblioteca Python que permite a comunicação direta com os servidores do Telegram, por meio do protocolo [MTProto](https://core.telegram.org/mtproto). Funciona usando a chamada <i>API do Telegram</i>, diferente da <i>API de Bots</i>. Apesar de a Pyrogram também servir para a criação de bots, não é possível afirmar que é vantajoso usar a <i>API do Telegram</i> em todos os casos, pois existem métodos exclusivos em cada uma das opções. Este texto, por tratar de uma automação em <b>conta de usuário</b>, tal API é a única alternativa.

<img src="https://docs.pyrogram.org/_static/img/mtproto-vs-bot-api.png" alt="Diferença entre a API de Bots e a API do Telegram." style="width:50%; background-color:#848383;border-radius:2%">
<br><small>Diferença entre a API de Bots e a API do Telegram. [Fonte](https://docs.pyrogram.org/topics/mtproto-vs-botapi)</small>

## Credenciais

Para gerar as credenciais necessárias para se comunicar aos servidores do Telegram, acesse [https://my.telegram.org/apps](https://my.telegram.org/apps). Digite seu telefone e clique em <i>Next</i>. Em seguida, digite o código recebido via Telegram. Para que tudo funcione serão necessários os valores de <b>App api_id</b> e <b>App api_hash</b>.

> <b>Estes dados são confidenciais. Jamais os compartilhe.</b>

# [OpenAI Whisper](https://github.com/openai/whisper)

A transcrição de textos é feita usando o modelo de reconhecimento de voz da OpenAI, chamado Whisper. É um modelo grátis e que funciona localmente, ou seja, não exige comunicação com nenhum servidor externo. Tudo acontece na sua máquina, resguardando sua privacidade.

O Whisper possui cinco modelos de reconhecimento de voz, que devem ser usados de acordo com a disponibilidade de recursos computacionais do servidor. [Leia mais no repositório oficial](https://github.com/openai/whisper#available-models-and-languages). O presente script usa o modelo <code>base</code>, porém, se possível, recomendo o uso do <code>small</code> para que as transcrições sejam mais exatas.

# Voice2Text

Respositório: [https://github.com/GabrielRF/Voice2Text](https://github.com/GabrielRF/Voice2Text)

Este pequeno script usa as credenciais da conta do Telegram para transcrever todas as mensagens de voz que forem recebidas em <b>conversas privadas</b>. A transcrição é automática, não exigindo nenhuma ação para que aconteça.

## Requisitos

Instale os requisitos usando o comando `pip install -r requirements.txt`. 

> Este passo é um pouco demorado devido ao tamanho da `openai-whisper`.

Caso queira instalar manualmente, os requisitos são:

```text
openai-whisper==20230314
TgCrypto==1.2.3
pyTelegramBotAPI==4.11.0
Pyrogram==2.0.106
```

## Voice2Text.py

<img src="/assets/img/voice2text.png" alt="Transcrição de áudio em conversa privada." style="width:80%;border-radius:2%">
<br><small>Transcrição de áudio em conversa privada.</small>

Ajustes os valores de `api_id` e `api_hash` nas linhas 7 e 8 do arquivo `Voice2Text.py`.

```python
from pyrogram import Client, filters, enums
import asyncio
import time
import os
import whisper

api_id = 123456789
api_hash = "1a2b3c4d5e6f7g8h9i0j"

app = Client("Voice2Text", api_id=api_id, api_hash=api_hash)
model = whisper.load_model("base")
```

Altere a linha 11 do arquivo caso queira utilizar um modelo diferente do <code>base</code>.

Por fim, execute o script usando o comando:

```shell
python Voice2Text.py
```

> Na primeira execução será necessário autenticar-se em sua conta do Telegram. Siga os passos e será criado um arquivo <code>.session</code>. Para remover a sessāo, remova este arquivo.

Para manter o processo em execução, leia o post ["Como manter um processo rodando para sempre"](/posts/Systemctl/).
{%endraw%}
