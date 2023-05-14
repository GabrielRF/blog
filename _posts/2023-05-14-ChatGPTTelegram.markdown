---
layout: post
title: "Bot de ChatGPT no Telegram"
date: 2023-05-14 16:30:00 -0300
categories: Telegram Bot
tags: telegram bot chatgpt openai
image: "/assets/img/ChatGPTTelegram.png"
---

## ChatGPT

"O <i>OpenAI ChatGPT</i> é uma ferramenta de inteligência artificial (IA) projetada pela OpenAI para gerar conversas humanas em tempo real. O ChatGPT usa um modelo de linguagem natural de última geração treinado em grandes quantidades de texto na internet para fornecer uma experiência de chatbot de alto nível. O sistema pode responder a perguntas, entender comandos e seguir tópicos de conversa, com habilidade suficiente para gerar respostas fluídas e coherentemente humanas. O OpenAI ChatGPT representa um passo significativo no futuro da IA e da comunicação humana" [^textobot].

## Bot no Telegram

Um bot no Telegram nada mais é que um programa de computador que segue uma receita de funcionamento, um algoritmo. Para funcionar é necessário que seu robô interaja com os servidores do Telegram. As instruções de como esta comunicação é feita é chamada de API, que pode ser entendida como uma interface de programação de aplicação, disponível em https://core.telegram.org/bots/api. [Leia mais](https://blog.gabrf.com/posts/HowToBot/)

## Requisitos

### Chave da OpenAI

A chave da OpenAI é facilmente gerada. Tendo uma conta no [site da OpenAI](https://platform.openai.com/overview), clique em *Personal* (canto superior direito) e, em seguida, em *View API Keys*. Crie uma chave clicando em *Create new secret key*.

### Token do bot

Para se comunicar com o Telegram, crie um bot, caso já não possua um, falando com o [@BotFather](https://t.me/BotFather). Envie o comando `/newbot` e siga as instruções.

## Instalação

Para criar o seu bot com ChatGPT, basta clonar o repositório [ChatGPT-Telegram-Bot](https://github.com/GabrielRF/ChatGPT-Telegram-Bot) e seguir o passo a passo do [post que explica como ter um bot na Amazon](https://blog.gabrf.com/posts/ServerlessBot/). Repare que o arquivo `serverless.yml` já está criado e pronto para ser usado.

Como o uso da API do ChatGPT é pago[^precoseplanos], é necessário definir quais identificadores do Telegram[^ids] serão respondidos pelo bot. Este controle é feito pela variável `USERID`, que pode conter uma lista de IDs de pessoas ou mesmo o ID de um grupo. Ou seja, no segundo caso, todos os membros do grupo serão capazes de interagir com o bot.

Consulte os preços e plano grátis de testes da API no [site oficial](https://openai.com/pricing).

Por fim, defina os valores das variáveis de ambiente necessárias para o funcionamento do bot.

```shell
export TOKEN=<TOKEN DO BOT>
export CHATGPTKEY=<CHAVE DO CHATGPT>
export USERID=<ID DE USUÁRIO>
```

## Execução

Finalmente, com todos os ajustes feitos, execute o comando abaixo para que o bot seja criado na Amazon.

```shell
serverless deploy
```

No fim da instalação, não esqueça de definir o webhook usando 

```shell
python setWebhook.py <URL DO BOT NA AMAZON>
```

Pronto. Seu bot deve estar funcionando.

<img src="/assets/img/ChatGPTTelegramBot.jpg" alt="Bot em funcionamento" style="width:50%">
<br><small>Bot em funcionamento</small>

## Código

### Função *ask_chatgpt* 

A função `ask_chatgpt` é responsável pelas consultas à API da OpenAI. É ela quem via a mensagem e recebe a resposta. A mensagem é enviada com `role: user`, indicando que são as mensagens enviadas pelo usuário. Já as respostas são armazenadas com `role: assistant`, pois são as respostas recebidas pelo assistente. É importante armazenar o histórico de mensagens para ser mantido o contexto da conversa. Caso o contexto não seja mantido (linhas [12](https://github.com/GabrielRF/ChatGPT-Telegram-Bot/blob/2570c9fbb8400c5d1998cf3dfe650af80e5d4473/bot.py#L12) e [20](https://github.com/GabrielRF/ChatGPT-Telegram-Bot/blob/2570c9fbb8400c5d1998cf3dfe650af80e5d4473/bot.py#L20)), a cada mensagem o bot irá se comportar como se fosse novo, como se não houvesse conversa anterior.

#### Parâmetros da OpenAI

* `model`: Identificador do modelo usado. Cada modelo possui um custo e uma finalidade específica. Valor padrão: `gpt-3.5-turbo`; 
* `messages`: Lista de mensagens contidas na conversa;
* `n`: Quantas respostas serão geradas. Valor padrão: `1`;
* `temperature`: Nível de aleatoriedade da resposta, variando de 0 a 2, sendo zero de maior aleatoriedade e dois de menor aleatoriedade. Valor padrão: `1`;
* `max_tokens`: O tamanho máximo em tokens usado em cada interação. Valor padrão `1024`.

Para mais informações, consulte a [API oficial](https://platform.openai.com/docs/api-reference/chat/create).

### Função *clean_context*

Ao receber o comando `/start` o bot irá limpar o histórico da conversa, perdendo todo o contexto que possui. Pode ser útil para fins de manter o custo do bot baixo.

### Função *message_received*

Todas as mensagens, com exceção do comando `/start`, entram nesta função, que irá chamar a função `ask_chatgpt` usando o conteúdo da mensagem recebida como parâmetro.

### Função *hello_http*

Função responsável pelo funcionamento do webhook.

Caso queira executar o bot usando **polling**, remova toda esta função e acrescente ao código a linha:

```python
bot.infinity_polling()
```

[^ids]: Leia mais em [https://blog.gabrf.com/posts/TelegramID/](https://blog.gabrf.com/posts/TelegramID/)

[^textobot]: Este texto foi elaborado pelo próprio bot usando o ChatGPT.

[^precoseplanos]: Consulte os preços e planos, além do plano grátis (de testes), no [site oficial](https://openai.com/pricing).
