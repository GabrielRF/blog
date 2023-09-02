---
layout: post
title: "Bot que cria legendas diretamente no Telegram"
date: 2023-09-02 14:00:00 -0300
categories: [Telegram, Bot]
tags: telegram bot whisper openai captions subtitles chatgpt
image: "/assets/img/VideoCaptionsBot.jpg"
---

# [VideoCaptionsBot](https://t.me/VideoCaptionsBot)

O VideoCaptionsBot foi criado com a mesma tecnologia das [transcrições automáticas de mensagens de voz](/posts/WhisperPyrogram/), usando a biblioteca [OpenAI-Whisper](https://github.com/openai/whisper). Ele recebe arquivos de vídeo ou mensagens de voz circulares (vídeo de bolinha) e cria, automaticamente, a legenda e, se necessário, a tradução.

Apesar da criação de legendas no idioma original do vídeo ser executada localmente, no servidor do bot, com a finalidade de evitar o mau uso do serviço o texto é submetido à [ferramenta de moderação da OpenAI](https://platform.openai.com/docs/guides/moderation). Caso seja identificado conteúdo não permitido, o bot removerá o arquivo da conversa e não enviará o vídeo legendado ao usuário.

Usando o ChatGPT, também da OpenAI, o bot é capaz de traduzir as legendas originais para o idioma do usuário. O VideoCaptionsBot considera o idioma ajustado no aplicativo Telegram como o idioma do usuário, portanto, sempre que uma legenda for gerada em idioma diferente do aplicativo, o bot fará o envio de dois vídeos, um com a legenda em idioma original e outro com a legenda no idioma do usuário.

<img src="/assets/img/RodrigoGoes.gif" alt="Legenda em vídeo do Rodrigo Goes." style="width:50%">
<br><small>Legenda em vídeo do Rodrigo Goes - [Canal no YouTube](https://www.youtube.com/@Rodrigo_Goes).</small>

## Funcionamento

O bot funciona em dois processos Python separados. O primeiro, `videocaptionsbot.py` tem a função de receber os comandos `/start`, `/termos`, `/pix` e de receber os vídeos. As mensagens contendo os vídeos são enviadas à fila, uma instância do RabbitMQ, e atendidas uma a uma no processo seguinte.

O arquivo `consumeline.py` verifica a existência de mensagens na fila e as processa. Inicialmente cria um arquivo `.srt` com as legendas e o submete à ferramenta de moderação. Caso haja aprovação, um novo vídeo contendo as legendas é gerado. Por fim, o bot verifica se o idioma do app do usuário é o mesmo da legenda criada e, caso não seja, solicita ao ChatGPT a tradução das legendas para que um segundo arquivo de vídeo seja gerado e enviado ao usuário.

## Código Fonte

O VideoCaptionsBot possui código aberto, disponível no repositório do GitHub de endereço [github.com/GabrielRF/VideoCaptionsBot](https://github.com/GabrielRF/VideoCaptionsBot).

Toda e qualquer colaboração é bem vinda! Críticas e sugestões podem ser feitas no próprio GitHub ou diretamente no Telegram. Grupo para conversarmos sobre o bot disponível [aqui](https://t.me/GabRF).
