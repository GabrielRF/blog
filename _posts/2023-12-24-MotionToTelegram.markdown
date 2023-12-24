---
layout: post
title: "Detecção de movimento e disparo de vídeo por Telegram"
date: 2023-12-24 13:00:00 -0300
categories: [Automação Residencial, CFTV]
tags: python hass homeassistant camera webcam cftv rpi raspberry telegram bot
image: "/assets/img/MotionToTelegram.png"
---
> Monitore sua casa com custo baixíssimo e sem se preocupar com DVR ou espaço em disco.

Mais que um monitor de movimento, uma <i>webcam</i> é uma poderosíssima ferramenta de automação residencial e de segurança. Usando duas soluções <i>open source</i> e um aplicativo de mensagens, este guia explica como fazer o envio de vídeos curtos sempre que for detectado movimento na câmera.

# Requisitos

## Motion

Programa de código aberto capaz de interpretar de vídeo de câmeras e agir quando houver movimento. É o componente responsável por fazer o monitoramento e gerar um arquivo `.mp4` sempre que houver movimento. Além disso, é capaz de fazer <i>streaming</i> da câmera, permitindo a visualização em tempo real do que está acontecendo, além de poder ser integrado ao [Home Assistant](https://blog.gabrf.com/categories/home-assistant/).

Site: [https://motion-project.github.io/](https://motion-project.github.io/)

## Video2Telegram

Pequeno script Python que monitora uma pasta e, havendo um arquivo novo, o envia pelo aplicativo Telegram. Pode fazer a conversão para <i>gif</i> (Video2Telegram) ou não (Document2Telegram), a depender da preferência.

Repositórios:

* Video2Telegram: [https://github.com/GabrielRF/Video2Telegram](https://github.com/GabrielRF/Video2Telegram)

* Document2Telegram: [https://github.com/GabrielRF/Document2Telegram](https://github.com/GabrielRF/Document2Telegram)

## Telegram

O aplicativo Telegram é a escolha ideal por ser leve e com armazenamento gratuito na nuvem para arquivos de até 2GB, sem limite de quantidade. Além disso, pode ser acessado de diversos dispositivos, não ficando dependente de um celular o tempo todo.

# Configuração

## Raspberry Pi

> Este guia foi pensado e testado em um <i>Raspberry Pi 3 Model B Rev 1.2</i> e em um <i>Raspberry Pi Zero 2 W</i>. Pode ser aplicado também em computadores com sistema operacional <i>Linux</i>, desde que compatível com o [Motion](#motion).

> Este guia foi feito usando o sistema <i>Raspberry Pi OS Lite</i> de 11 de Dezembro de 2023. A vantagem da versão <i>Lite</i> é ser mais leve, consumindo menos espaço e recursos computacionais. Como o <i>Raspberry Pi</i> ficará dedicado à solução, sem <i>softwares</i> adicionais, a versão <i>Lite</i> foi a escolhida.

Baixe o sistema operacional de sua preferência no site oficial, em [https://www.raspberrypi.com/software/operating-systems/](https://www.raspberrypi.com/software/operating-systems/).

Feito o download, crie um disco de <i>boot</i> da imagem baixada.

<img src="/assets/img/MotionToTelegramImage.png" alt="Criação de disco de boot." style="width:80%">
<br><small>Criação de disco de boot.</small>

### Acesso SSH

Para habilitar o acesso via <i>ssh</i>, após a criação da imagem, crie um arquivo chamado `ssh` na partição de <i>boot</i>.

```shell
cd /media/gabriel/bootfs
touch ssh
```

> Substituindo `gabriel` pelo usuário de seu computador.

### Criação de usuário

Por padrão, o sistema operacional do <i>Raspberry Pi</i> exige a criação de um novo usuário ao ser ligado pela primeira vez. Para pular este passo, recomendo os passos abaixo:

#### Encripte a senha do usuário

Execute o comando abaixo e armazene seu resultado, pois será usado a seguir.

```shell
echo '<DIGITE SUA SENHA AQUI>' | openssl passwd -6 -stdin
```

#### Crie um arquivo na partição de boot

```shell
touch userconf
```

Abra o arquivo `userconf` criado acima com o editor de sua preferência e cole o conteúdo

```text
<USUARIO>:<SENHA ENCRIPTADA>
```

Ou seja, o arquivo `userconf` com o usuário `gabriel` e a senha `teste` teria, por exemplo, o conteúdo:
```text
gabriel:$6$6OITwZhScE7vD4r7$bEGFbmrOqbVCEGp.N5rmu9ca1arqH.ynOxRXMYl6XYLeyXfPIEMaOGr0yr9tY8RKcSBJDEfXHUYva3QWRTR2G.
```

> Atenção: A mesma senha não irá gerar a mesma saída! Não estranhe se o resultado for diferente do exemplo.

### Rede

Infelizmente não é mais possível fazer a configuração da conexão à redes sem fio pelo arquivo `wpa_supplicant.conf`[^wpa], portanto, para seguir este guia, recomendo que faça a ligação à sua rede via cabo.

[^wpa]: [https://www.raspberrypi.com/documentation/computers/configuration.html#connect-to-a-wireless-network](https://www.raspberrypi.com/documentation/computers/configuration.html#connect-to-a-wireless-network).

### Configuração do Raspberry Pi OS

Feita a configuração inicial, acesse seu <i>Raspberry Pi</i> via <i>ssh</i>.

```bash
ssh <USUARIO>@<IP RASPBERRY PI>
```

Atualize os pacotes e instale o Motion.

```bash
sudo apt-get update
sudo apt-get install motion
```

O Motion irá falhar caso tente executá-lo. Para corrigir o problema, execute os dois comandos:

```bash
sudo mkdir motion:motion /var/log/motion/
sudo chown -R motion:motion /var/log/motion/
```

O primeiro cria a pasta de <i>log</i> e o segundo dá a permissão correta para esta pasta. Feito isto, o Motion já está pronto para ser usado.

### Configuração do Motion

O arquivo de configuração do Motion fica armazenado em `/etc/motion/motion.conf`. Abaixo listo alguns pontos relevantes e algumas recomendações de mudanças. Por ser um ajuste fino que depende de diversos fatores, como modelo da câmera, intensidade da luz e área monitorada, não existe resposta 100% correta. Cada caso necessita de ajustes específicos.

* `target_dir`: Pasta em que ficam os vídeos e fotos do Motion;
* `video_device`: Dispositivo de vídeo. Em geral, não precisa ser alterado;
* `width`: Largura da imagem. Quanto maior, melhor. Porém, quanto maior, mais pesado o arquivo. Recomendo ao menos `800`;
* `height`: Altura da imagem. Recomendo ao menos `600`;
* `framerate`: Taxa de quadros por segundo. Recomendo `30`;
* `text_left`: Texto exibido na esquerda;
* `text_right`: Texto da direita;
* `threshold`: Quantidade mínima de <i>pixels</i> que precisam variar para que seja considerado movimento;
* `minimum_motion_frames`: Número mínimo de quadros com variação para ser considerado movimento;
* `event_gap`: Intervalo em segundos entre eventos. Recomendo `0`;
* `pre_capture`: Número de quadros anteriores ao movimento para compor o vídeo. Recomendo `2`;
* `post_capture`: Número de quadros posteriores ao movimento para compor o vídeo. Recomendo `2`;
* `movie_max_time`: Tamanho máximo do vídeo em segundos. Recomendo `30`;
* `movie_quality`: Qualidade do vídeo. Recomendo `100`;
* `movie_codec`: <i>Coded</i> do vídeo. Recomendo `mp4`[^mp4];
* `movie_filename`: Nome do arquivo de vídeo;
* `webcontrol_localhost`: Permitir controle de fora do <i>Raspberry Pi</i>. Recomendo `off`;
* `stream_localhost`: Permitir acesso ao streaming de fora do <i>Raspberry Pi</i>. Recomendo `off`.

Após toda e qualquer mudança no arquivo `motion.conf` é necessário reiniciar a aplicação executando:

```bash
sudo systemctl restart motion
```

[^mp4]: Recomendo o uso do <i>codec</i> `mp4` por ser um formato que funciona bem com o Telegram. Além disso, caso o vídeo não tenha som, o Telegram automaticamente transforma o `mp4` em `gif`.

#### Registros / Log

Para ver o log, acesse o arquivo `/var/log/motion/motion.log`. Caso necessite visualizar mais detalhes, altere o valor de `log_level` para `9` no arquivo de configuração.

#### Streaming

Caso tenha permitido a visualização da câmera no parâmetro `stream_localhost`, acesse o endereço IP do <i>Raspberry Pi</i> pelo seu navegador pela porta `8080`.

<img src="/assets/img/MotionToTelegramStreaming.png" alt="Acesso ao streaming da camera." style="width:80%">
<br><small>Acesso via browser ao streaming da camera.</small>

Clicando-se na câmera é possível capturar o endereço direto de seu <i>stream</i>, que pode ser usado no [Home Assistant](/tags/homeassistant/) ou em qualquer outro software compatível.

### Video2Telegram

#### Requisitos

```text
inotify==0.2.10
pyTelegramBotAPI==4.11.0
```

#### Arquivo Python

```python
import inotify.adapters
import os
import telebot

TOKEN = <TOKEN DO BOT>
FOLDER = <PASTA A SER MONITORADA>
EXTENSION = <EXTENSÃO DO ARQUIVO>
DESTINATION = <ID DE DESTINO>

bot = telegram.Bot(TOKEN)
notifier = inotify.adapters.InotifyTree(FOLDER)

for event in notifier.event_gen():
    if event is not None:
        if 'IN_CLOSE_WRITE' in event[1] and EXTENSION in event[3]:
            file_path = event[2] + '/' + event[3]
            file_open = open(file_path, 'rb')
            try:
                bot.send_chat_action(DESTINATION, 'upload_video')
                bot.send_animation(DESTINATION, file_open)
            except:
                bot.send_chat_action(DESTINATION, 'upload_document')
                bot.send_document(DESTINATION, file_open)
            os.remove(file_path)
```

##### Variáveis

* `TOKEN`: Token do bot[^bot];
* `FOLDER`: Pasta a ser monitorada. Valor padrão: `/var/lib/motion`;
* `EXTENSION`: Extensão de arquivo a ser enviada. Recomendado: `mp4`
* `DESTINATION`: Destino da mensagem no Telegram[^id].

[^bot]: [Como fazer bots para o Telegram](/posts/HowToBot/).
[^id]: [Como obter IDs no Telegram](/posts/TelegramID/).

##### Envio de gif ou Envio de documento

O código, como mostrado acima, primeiro tenta enviar um <i>gif</i> e, caso falhe, tenta enviar o documento. Isto é, primeiro há a tentativa de conversão do vídeo em gif e, caso ocorra uma falha, o arquivo é enviado sem alteração. Caso prefira o envio sem conversão alguma, remova as linhas 17, 18, 19 e 20, ou seja, remova do código o trecho que contém:

```python
            try:
                bot.send_chat_action(DESTINATION, 'upload_video')
                bot.send_animation(DESTINATION, file_open)
            except:
```

Ajuste a identação, alinhando as duas linhas iniciadas com `bot.` à linha iniciada com `file_open`.

A última linha, `os.remove(gif_path)`, remove o arquivo após o envio, evitando que o disco do computador fique cheio. 

Por fim, execute o script python[^runforever].

[^runforever]: [Como manter um processo rodando para sempre](/posts/Systemctl/).

## Câmeras

As câmeras que podem ser usadas com esta solução são as câmeras que funcionam como <i>webcam</i>, câmeras comuns, geralmente que se conectam via porta USB. Para testes foram usadas três:

* Logitech C920;
* Logitech C120;
* Camera V2 para Raspberry Pi.

Todas com resultados satisfatórios, variando apenas a qualidade da imagem de uma câmera para a outra.

---
