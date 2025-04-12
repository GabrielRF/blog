---
layout: post
title: "Moderação automática de conteúdo usando a nuvem da OpenAI"
date: 2025-04-12 18:30:00 -0300
categories: [Telegram, Bot]
tags: telegram bot openai moderation
image: "/assets/img/AnaliseDeConteudoBot.jpg"
---

# Moderação

A OpenAI oferece uma ferramenta gratuita de moderação[^moderation], cuja finalidade é analisar os conteúdos e classificá-los entre as categorias:

* _harassment_: Conteúdo que expresse, incite ou promova linguagem de assédio contra qualquer pessoa;
* _harassment/threatening_: Conteúdo de assédio que também inclua violência ou dano grave contra qualquer pessoa;
* _hate_: Conteúdo que expresse, incite ou promova ódio com base em raça, gênero, etnia, religião, nacionalidade, orientação sexual, deficiência ou casta;
* _hate/threatening_: Conteúdo de ódio que também inclua violência ou dano grave contra o grupo alvo com base em raça, gênero, etnia, religião, nacionalidade, orientação sexual, deficiência ou casta;
* _illicit_: Conteúdo que ofereça conselhos ou instruções sobre como cometer atos ilícitos.
* _illicit/violent_: Os mesmos tipos de conteúdo sinalizados pela categoria _illicit_:, mas também inclui referências à violência ou à obtenção de armas;
* _self-harm_: Conteúdo que promove, incentiva ou retrata atos de automutilação, como suicídio, automutilação e transtornos alimentares;
* _self-harm/intent_: Conteúdo em que o falante expressa que está se envolvendo ou pretende se envolver em atos de automutilação, como suicídio, automutilação e transtornos alimentares;
* _self-harm/instructions_: Conteúdo que incentiva a prática de atos de automutilação, como suicídio, automutilação e transtornos alimentares, ou que fornece instruções ou conselhos sobre como cometer tais atos;
* _sexual_: Conteúdo destinado a despertar excitação sexual, como a descrição de atividade sexual, ou que promove serviços sexuais (exceto educação sexual e bem-estar);
* _sexual/minors_: Conteúdo sexual que inclua um indivíduo com menos de 18 anos;
* _violence_: Conteúdo que retrate morte, violência ou lesão física;
* _violence/graphic_: Conteúdo que retrate morte, violência ou lesão física em detalhes gráficos.

A resposta da ferramenta é no formato de probabilidades.

## Envio de frase

```python
from openai import OpenAI

OPENAI_KEY=open('utils/openai.conf', 'r').read().strip()
client = OpenAI(api_key=OPENAI_KEY)

response = client.moderations.create(
    model="omni-moderation-latest",
    input="Você é feio",
)

print(response)
```

## Resposta

```json
ModerationCreateResponse(id='modr-970d409ef3bef3b70c73d8232df86e7d',
model='omni-moderation-latest',
results=[
    Moderation(
        categories=Categories(
            harassment=True,
            harassment_threatening=False,
            hate=False,
            hate_threatening=False,
            self_harm=False,
            self_harm_instructions=False,
            self_harm_intent=False,
            sexual=False,
            sexual_minors=False,
            violence=False,
            violence_graphic=False,
            harassment/threatening=False,
            hate/threatening=False,
            illicit=False,
            illicit/violent=False,
            self-harm/intent=False,
            self-harm/instructions=False,
            self-harm=False,
            sexual/minors=False,
            violence/graphic=False
        ),
        category_scores=CategoryScores(
            harassment=0.8874508716149667,
            harassment_threatening=4.583129006350382e-05,
            hate=0.0071335892195802525,
            hate_threatening=7.646537350121258e-07,
            self_harm=0.00046372467876705764,
            self_harm_instructions=0.00021833860887263772,
            self_harm_intent=0.0002152775092824918,
            sexual=7.437028637763686e-05,
            sexual_minors=1.8925148246037342e-06,
            violence=0.0005005862322300009,
            violence_graphic=3.2192334497037803e-06,
            harassment/threatening=4.583129006350382e-05,
            hate/threatening=7.646537350121258e-07,
            illicit=2.0342697805520655e-05,
            illicit/violent=2.01456908529364e-06,
            self-harm/intent=0.0002152775092824918,
            self-harm/instructions=0.00021833860887263772,
            self-harm=0.00046372467876705764,
            sexual/minors=1.8925148246037342e-06,
            violence/graphic=3.2192334497037803e-06
        ),
        flagged=True,
        category_applied_input_types={
            'harassment': ['text'],
            'harassment/threatening': ['text'],
            'sexual': ['text'],
            'hate': ['text'],
            'hate/threatening': ['text'],
            'illicit': ['text'],
            'illicit/violent': ['text'],
            'self-harm/intent': ['text'],
            'self-harm/instructions': ['text'],
            'self-harm': ['text'],
            'sexual/minors': ['text'],
            'violence': ['text'],
            'violence/graphic': ['text']
        }
    )
])
```

## Resultado

A linha 49 confirma que o conteúdo resultou em __positivo__ para alguma das categorias:
```
flagged=True,
```
Já a linha 6 indica:
```
harassment=True,
```
Com a probabilidade conforme a linha 28:
```
harassment=0.8874508716149667,
```

Ou seja, a ferramenta detectou um assédio(_harassment_) com probabilidade de 88.74% na mensagem enviada.

## Envio de imagem

Para fazer a análise de imagens, utilize o código:

```python
from openai import OpenAI

OPENAI_KEY=open('utils/openai.conf', 'r').read().strip()
client = OpenAI(api_key=OPENAI_KEY)

response = client.moderations.create(
    model="omni-moderation-latest",
    input=[
        {
            "type": "image_url",
            "image_url": {
                <URL_DA_IMAGEM>
            }
        }
    ]
)

print(response)
```

# Telegram

Os conteúdos enviados no Telegram podem ser nocivos e ocasionar a remoção do grupo uma vez que os Termos de Uso foram descumpridos. Para evitar este tipo de problema, utilizo um bot que submete à OpenAI textos e imagens enviados e, dependendo do caso, notifica os administradores ou remove o conteúdo de imediato. Assim, não é necessário que um administrador esteja sempre observando tudo. O bot irá alertar sobre textos ou remover imagens quando for necessário.

## Bot

O AnaliseDeConteudoBot submete à ferramenta todo o conteúdo compartilhado em grupos em que sou administrador. Caso o conteúdo seja um texto, todos os administradores do grupo recebem uma mensagem com o alerta, contendo o link para a mensagem original e uma cópia do que foi escrito. Caso o conteúdo seja uma imagem, o bot automaticamente a remove do grupo após notificar os administradores.

> Após meses usando a ferramenta, cheguei à conclusão que o valor de `0.8` é um bom limite para notificar os administradores ou remover conteúdos. Menos que isso gerou muitos falsos positivos.

Para ficar claro para o grupo que os administradores foram alertados, as mensagens que gerarem notificações recebem a reação 🤔. Caso um dos administradores clique em _Ignorar alerta_, a reação é removida da mensagem. O bot também faz a verificação de mensagens editadas, portanto, é possível que uma mesma mensagem gere alertas repetidos a depender do conteúdo de cada edição.

<img src="/assets/img/AnaliseDeConteudoBot_alerta.png" alt="Exemplos de alertas de conteúdos inapropriados." style="width:50%">
<br><small>Exemplos de alertas de conteúdos inapropriados categorizados como _harassment_.</small>

## Executando o bot

Todo o conteúdo do bot está presente no repositório [github.com/GabrielRF/AnaliseDeConteudoBot](https://github.com/GabrielRF/AnaliseDeConteudoBot). Após baixar o conteúdo, abra a pasta `utils` e edite os três arquivos, sendo eles:
* `bottoken.conf`: Com o token do Bot obtido no [@BotFather](https://t.me/BotFather);
* `groups.conf`: Os IDs dos grupos em que o bot irá funcionar[^ids];
* `openai.conf`: O token de acesso obtido na [OpenAI](https://auth.openai.com/log-in).

Preenchidos os arquivos corretamente, basta executar o arquivo `bot.py` como preferir[^systemctl].

---

[^moderation]: [OpenAI Moderation](https://platform.openai.com/docs/guides/moderation)
[^ids]: [Como obter IDs no Telegram](/posts/TelegramID/)
[^systemctl]: [Como manter um processo rodando para sempre](/posts/Systemctl/)
