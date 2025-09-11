---
layout: post
title: "Monitoramento do Diário Oficial da União via Telegram"
date: 2025-05-24 18:25:00 -0300
categories: [Telegram, Bot]
tags: telegram bot dou diariooficial
image: "/assets/img/dou_logo.jpg"
---

# Diário Oficial da União

> O Diário Oficial da União é um dos veículos de comunicação pelo qual a Imprensa Nacional do Brasil tem de tornar público todo e qualquer assunto acerca do âmbito federal.[^wikipedia]

Acompanhar o DOU é uma tarefa tediosa, mas que deve ser feita diariamente, seja você um servidor público, um concurseiro ou apenas um cidadão que gosta de saber das decisões que dizem respeito ao nosso país. Não raramente são publicadas versões adicionais e em horários não programados. Fazer o montoramento manualmente é inviável.

# DOU2Telegram

## requirements.txt

Instale os seguintes requisitos:

```text
pyTelegramBotAPI==4.21.0
requests==2.31.0
urllib3==1.26.3
beautifulsoup4==4.9.1
```

## bot.py

Preencha as variáveis e execute o código:

* `<TOKEN>`: Token do bot que fará os envios[^HowToBot].
* `<TERMO>`: Termo usado na busca do Diário Oficial.
* `<CHATID>`: ID de destino da mensagem[^TelegramID].

```python
import requests
import telebot
import urllib
from bs4 import BeautifulSoup
from datetime import datetime

bot = telebot.TeleBot('<TOKEN>')
termo = '<TERMO>'
chatid = <CHATID>

def check_dou(link, param):
    try:
        response = requests.get(
            link,
            timeout=10,
                headers={'User-agent': 'Mozilla/5.1'}
            )
    except:
        return False
    return BeautifulSoup(response.content, 'html.parser')

def get_dou_results(html):
    div_content = html.find('p', {'class': 'search-total-label'})
    qtd_results = int(div_content.text.strip().split()[0])
    return div_content, qtd_results

def send_message(param, link, results):
    btn_link = telebot.types.InlineKeyboardMarkup()
    btn = telebot.types.InlineKeyboardButton(f'🔗 Consultar', url=link)
    btn_link.row(btn)
    bot.send_message(
        chatid,
        '🛎  <b>Diário Oficial da União</b>\n\n'+
        f'<b>Termo</b>: <code>{param}</code>\n'+
        f'<b>Resultados</b>: {results}\n',
        parse_mode='HTML',
        reply_markup=btn_link
    )

if __name__ == "__main__":
    today = datetime.today().strftime("%d-%m-%Y")
    link = (
        'https://www.in.gov.br/consulta/-/buscar/dou?q=%22'
        +f'{urllib.parse.quote_plus(termo)}'
        +'%22&s=todos&exactDate=personalizado'
        +f'&sortType=0&publishFrom={today}&publishTo={today}'
    )
    html = check_dou(link, termo)
    if html:
        content, results = get_dou_results(html)
        if results:
            send_message(termo, link, results)
```

## Resumo do funcionamento

O script simula uma busca no Diário Oficial usando como parâmetro a data atual, função `check_dou`. Havendo resultados, função `get_dou_results`, é enviada uma mensagem com a função `send_message`.

---
[^wikipedia]: [https://pt.wikipedia.org/wiki/Di%C3%A1rio_Oficial_da_Uni%C3%A3o](https://pt.wikipedia.org/wiki/Di%C3%A1rio_Oficial_da_Uni%C3%A3o)
[^HowToBot]: [Como fazer bots para o Telegram](/posts/HowToBot).
[^TelegramID]: [Como obter IDs no Telegram](/posts/TelegramID).
