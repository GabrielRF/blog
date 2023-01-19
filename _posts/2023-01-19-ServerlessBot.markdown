---
layout: post
title: "Como hospedar um bot na Amazon AWS"
date: 2023-01-19 19:20:00 -0300
categories: Telegram Bot
tags: telegram bot aws serverless webhook
image: "/assets/img/serverlessbot.png"
---

<center>
<img src="/assets/img/serverlessbot.png" alt="Imagens: AWS Lambda, Bot e Serverless" style="width:50%">
<br><small>Imagens: AWS Lambda, Bot e Serverless</small>
</center>

## Amazon Lambda

Lambda é a hospedagem de funções do tipo "_serverless_" da Amazon. Estas funções não exigem o gerenciamento de recursos computacionais ou qualquer tipo de infraestrutura para funcionarem. Basta enviar o código fonte e a Amazon se encarregará de sua execução conforme os critérios definidos. O código fica "adormecido" e somente é executado quando chamado.

## Serverless.com

Serverless é uma ferramenta que facilita o envio e o controle de soluções na nuvem para os mais diversos provedores. A configuração é toda feita em um arquivo `.yml` e tudo é gerenciado facilmente.

### Instalação

Para instalar a ferramenta serverless, siga as [instruções do site oficial](https://www.serverless.com/framework/docs/getting-started) ou do [canal oficial no YouTube](https://www.youtube.com/watch?v=NjZaXwNU08Q).

### Definir e Configurar Chave de acesso

Feita a instalação, crie uma chave de acesso para o serverless conforme a [documentação oficial](https://www.serverless.com/framework/docs/providers/aws/guide/credentials) ou o [vídeo](https://www.youtube.com/watch?v=KngM5bfpttA). O comando para o ajuste das credenciais é o seguinte:

```shell
serverless config credentials \
  --provider aws \
  --key <Access key ID> \
  --secret <Secret access key>
```

### Arquivo serverless.yml

Crie o arquivo `serverless.yml` com o conteúdo abaixo, ajustando-o conforme a necessidade:

```yaml
service: <nome>
frameworkVersion: '3'

provider:
  name: aws
  runtime: python3.8
  region: eu-west-3

functions:
  bot:
    handler: bot.hello_http
    events:
      - httpApi:
         path: /
         method: post
    environment:
      TOKEN: ${env:TOKEN}

plugins:
  - serverless-python-requirements

custom:
  pythonRequirements:
    dockerizePip: false
    usePipenv: false
```

O que não for listado abaixo __não deve ser alterado__:

* `service`: Nome do bot (sem espaços);
* `region`: Região em que o bot será hospedado. Recomendo `eu-west-3` por ser na Europa, deixando o bot com baixa latência;
* `handler`: Composto por `<nome_do_arquivo_py>.<função_que_recebe_requisições>`;
* `environment`: Variáveis de ambiente que serão usadas pelo `.py`;
* `plugins`: Recomendo usar o `serverless-python-requirements` para que sejam instaladas as dependências listadas no `requirements.txt`.

Para instalar o plugin, use o comando:

```shell
sls plugin install -n serverless-python-requirements
```

Caso não utilize o `requirements.txt`, remova as linhas 19 em diante do `serverless.yml`.

## Bot

### Arquivo bot.py

Tendo o bot no arquivo `bot.py`, um exemplo de robô que repete tudo o que recebe seria:

```python
import telebot
import os

TOKEN = os.getenv('TOKEN')
bot = telebot.TeleBot(TOKEN, threaded=False)

@bot.message_handler(func=lambda m:True)
def on_message(message):
    bot.reply_to(message, message.text)

def hello_http(event, context):
    update = telebot.types.Update.de_json(event['body'])
    try:
        bot.process_new_updates([update])
    except:
        pass
    return {"statusCode": 200, "body": "hello world"}
```

A função `hello_http` deve ter o mesmo nome usado no arquivo `serverless.yml`. É ela que receberá os _requests_ enviados ao bot. Ou seja, este bot usa `webhook` em vez de `polling`. Isto significa que o bot é executado somente quando chamado. Ele não busca atualizações no servidor do Telegram, o Telegram é que vai até ele quando algo chegar.

### Arquivo requirements.txt

O `requirements.txt` seria:

```text
pyTelegramBotAPI==4.6.0
```

Caso haja necessidade, basta acrescentar outras bibliotecas a serem instaladas, uma por linha e, preferencialmente, com as versões definidas.

### Comando Deploy

Execute o comando abaixo para armazenar o token em uma variável de ambiente, substituindo `<TOKEN_DO_BOT>` pelo valor fornecido pelo BotFather:

```shell
export TOKEN=<TOKEN_DO_BOT>
```

Em seguida, execute o comando abaixo para fazer o deploy:

```shell
serverless deploy
```

Após a conclusão, copie o valor do `endpoint`, pois ele será usado no [próximo passo](#arquivo-setwebhookpy).

### Arquivo setWebhook.py

> Este arquivo será usado somente uma vez, após a primeira execução do [Deploy](#comando-deploy). Não é necessário executá-lo outras vezes a menos que o `ENDPOINT` seja alterado, o que não é comum.

Crie o arquivo `setWebhook.py` exatamente como abaixo:

```python
import os
import sys
import telebot

TOKEN = os.getenv('TOKEN')
WEBHOOK = sys.argv[1]

bot = telebot.TeleBot(TOKEN)

bot.remove_webhook()
bot.set_webhook(WEBHOOK)
```

Usando o valor do `endpoint` fornecido anteriormente, execute:

```shell
python setWebhook.py "<ENDPOINT>"
```

Repare que as `"` (aspas) devem ser usadas no comando!

__Pronto!__ Definido o webhook seu bot já estará funcionando.

### Comando Remove

Para remover tudo de uma só vez, basta executar:

```shell
serverless remove
```
