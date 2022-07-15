---
layout: post
title: "Bot do PromoPassagens automático e grátis!"
date: 2022-06-12 12:00:00 -0300
categories: [Telegram, Bot]
tags: telegram bot canal automação github
image: "/assets/img/promopassagens.jpg"
---

> Automação do canal PromoPassagens com posts de lista do Twitter.

<center>
<img src="/assets/img/promopassagens.jpg" alt="Logo PromoPassagens" style="width:50%; border-radius:50%"> 
<br><small><a href="https://t.me/PromoPassagens">PromoPassagens</a></small>
</center>

O canal PromoPassagens reúne as postagens de uma lista do Twitter em um canal do Telegram de forma automática. Os posts são formatados para um padrão e as cidades recebem `#hashtags` para facilitar buscas.

__Link do repositório:__ [github.com/GabrielRF/PromoPassagens](https://github.com/GabrielRF/PromoPassagens/)

## Funcionamento

O código e a automação são hospedados no GitHub e não dependem de nenhum serviço externo. A cada quinze minutos é executada uma ação do GitHub que se encarrega de todo o processo automaticamente. O repositório pode ser público ou privado, com a diferença que repositórios privados possuem uma limitação na quantidade de execuções. Para mais informações, [verifique aqui as condições dos planos](https://github.com/pricing#compare-features).

## Configuração

### Ação

A ação é definida no arquivo `.github/workflows/cron.yml`.

* `workflow_dispatch` permite que a ação seja executada manualmente caso necessário, sem precisar ficar aguardando o intervalo;
* `cron: '*/15 * * * *'` define que a ação deve ser executada a cada quinze minutos. Não recomendo usar intervalos menores que 10 minutos;
* `actions/checkout@v2` faz a ação obter o código do repositório;
* `actions/setup-python@v2` instala python na máquina que executa a ação;
* `run: pip install -r requirements.txt` instala as bibliotecas necessárias para o script python funcionar;
* `run: python promopassagens.py` executa o script que faz todo o processo;
  * `env` permite que o script visualize as variáveis de ambiente definidas como *secrets*.

### Arquivos de configuração

As cidades são listadas em `cities.txt`. Cada cidade deve ocupar uma linha.

O arquivo `blocklist.txt` possui uma lista de palavras que fazem a postagem ser ignorada. Basta uma delas estar presente para que a mensagem não vá para o canal.

### Variáveis de ambiente

Nas configurações do repositório acesse `Secrets` > `Actions`. Crie as variáveis:

* `BOT_TOKEN`: Token do bot que enviará as mensagens no canal. Fornecido pelo [@BotFather](https://t.me/BotFather);
* `DESTINATION`: Canal público que receberá as mensagens. Exemplo: `PromoPassagens` (não use `@`!);
* `TWITTER_ACCESS_SECRET`: Valor fornecido pelo [Twitter](https://developer.twitter.com/en/portal/dashboard);
* `TWITTER_ACCESS_TOKEN`: Valor fornecido pelo [Twitter](https://developer.twitter.com/en/portal/dashboard);
* `TWITTER_CONSUMER_KEY`: Valor fornecido pelo [Twitter](https://developer.twitter.com/en/portal/dashboard);
* `TWITTER_CONSUMER_SECRET`: Valor fornecido pelo [Twitter](https://developer.twitter.com/en/portal/dashboard);
* `TWITTER_OWNER`: Perfil criador da lista no Twitter. Exemplo: `GabRF`;
* `TWITTER_SLUG`: Slug da lista no Twitter. Exemplo: `canal-promopassagens`.

## Limitação

Infelizmente não é possível usar esta solução em canais privados. Toda sugestão é bem vinda!
