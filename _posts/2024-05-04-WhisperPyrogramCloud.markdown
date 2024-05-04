---
layout: post
title: "Transcrição automática de áudios na nuvem da OpenAI"
date: 2024-05-04 11:00:00 -0300
categories: [Telegram, Automação]
tags: telegram automação whisper openai pyrogram
image: "/assets/img/WhisperPyrogram.jpg"
---

{%raw%}
> Leia as informações introdutórias deste post em [Transcrição automática de mensagens de voz no Telegram](https://blog.gabrf.com/posts/WhisperPyrogram/).

# Nuvem da OpenAI

Usar a nuvem da OpenAI para a transcrição de áudios, apesar de paga, pode ser uma ótima alternativa quando avaliada a relação custo x benefício. Atualmente, em maio de 2024, a transcrição, utilizando o modelo *Whisper*, custa aproximadamente US$ 0,006 por minuto, arredondado para o segundo mais próximo do tempo do áudio. Tal custo, dependendo do uso, pode ser inferior ao gasto mensal com a energia elétrica para se manter um computador em funcionamento, pricipalmente quando comparado ao uso de máquinas com placas de vídeo de alto desempenho. [Veja os preços atualizados da nuvem da OpenAI no site oficial](https://openai.com/pricing).

Para meu uso pessoal, as principais vantages de usar a nuvem são:
* Menor gasto mensal, pois meu computador faz minha conta de energia elétrica aumentar aproximadamente R$ 100,00/mês, sendo o uso da nuvem inferior a US$ 2,00/mês;
* O servidor do script tem menores requisitos computacionais uma vez que não faz nenhum processamento importante.
* Excelente qualidade na transcrição.

# Mudanças no código

A mudança no código é pequena. Em resumo, basta alterar a função `voice_to_text`:

```python
def voice_to_text(voice_file, i=0):
    if i == 5:
        return False
    audio_file = open(voice_file, 'rb')
    try:
        result = client.audio.transcriptions.create(
            model="whisper-1",
            file=audio_file
        ).text
    except:
        time.sleep(3)
        result = voice_to_text(voice_file, i+1)
    return result
```

E criar um `client` antes de a função ser executada:

```python
client = OpenAI(api_key='ab-cdefghijklmnopqrstuvxz')
```

A `API_KEY` deve ser obtida cadastrando-se em [platform.openai.com/api-keys](https://platform.openai.com/api-keys).

O código completo da solução é:
```python
from pyrogram import Client, filters, enums
import telebot
import time
import os
from openai import OpenAI

api_id = 123456789
api_hash = "1a2b3c4d5e6f7g8h9i0j"

app = Client("Voice2Text", api_id=api_id, api_hash=api_hash)
client = OpenAI(api_key='ab-cdefghijklmnopqrstuvxz')

def remove_file(voice_file):
    os.remove(voice_file)

def voice_to_text(voice_file, i=0):
    if i == 5:
        return False
    audio_file = open(voice_file, 'rb')
    try:
        result = client.audio.transcriptions.create(
            model="whisper-1",
            file=audio_file
        ).text
    except:
        time.sleep(3)
        result = voice_to_text(voice_file, i+1)
    return result

def download_voice(message):
    return app.download_media(message)

def edit_message(client, msg, message, transcription):
    text = (
        f'<b>🎙   Transcrição automática:</b>'
        f'\n<i>{transcription}</i>'
    )
    client.edit_message_text(
        chat_id=message.chat.id,
        message_id=msg.id,
        text=text,
        parse_mode=enums.ParseMode.HTML
    )

def send_message(client, message):
    msg = app.send_message(
        chat_id=message.chat.id,
        text='📝 Transcrição iniciada...',
        parse_mode=enums.ParseMode.HTML,
        reply_to_message_id=message.id
    )
    return msg

def delete_message(client, msg):
    app.delete_messages(
        chat_id=msg.chat.id,
        message_ids=msg.id
    )

@app.on_message(filters.private & (filters.voice | filters.video_note))
def receive_voice(client, message):
    msg = send_message(client, message)
    app.send_chat_action(message.chat.id, enums.ChatAction.TYPING)
    voice_file = download_voice(message)
    transcription = voice_to_text(voice_file)
    if not transcription:
        delete_message(client, msg)
        return
    print(f'{message.chat.id} disse: {transcription}')
    edit_message(client, msg, message, transcription)
    remove_file(voice_file)

app.run()
```
{%endraw%}

**E pronto!** Deixando esse script em execução, as transcrições das mensagens de voz enviadas ou recebidas acontecerá automaticamente.

Leia mais em *[Como manter um processo rodando para sempre](https://blog.gabrf.com/posts/Systemctl/)*.
