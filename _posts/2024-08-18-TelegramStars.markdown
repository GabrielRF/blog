---
layout: post
title: "Telegram Stars - Primeiras impressões"
date: 2024-08-19 18:30:00 -0300
categories: [Telegram, Bot]
tags: telegram stars bot
image: "/assets/img/TelegramStars.png"
---

# Stars

No dia 6 de Junho de 2024 o Telegram lançou sua criptomoeda chamada <i>Telegram Stars</i>[^telegramblog]. Seu uso passou a ser <b>obrigatório</b> em transações que envolvam produtos ou serviços digitais[^starstos]. Bots que descumprirem a regra podem ser punidos com suspensão. 

## Compra

As <i>Stars</i> podem ser adquiridas a partir de qualquer bot que tenha sistema de cobrança. Basta clicar no botão de comprar que, não havendo saldo suficiente em <i>Stars</i>, será exibida a opção de compra. Seu custo é de aproximadamente US$ 1,00 para cada 50 <i>Stars</i>.

### Tabela de preços (em dólares)

> Sabendo que as estrelas possuem um valor fixo para o saque, a Taxa que o Telegram cobra em cada transação foi estimada nas tabelas abaixo.

#### Lojas

> AppStore e PlayStore

|Quantidade|Preço|Comissão Loja|Taxa Telegram|
|----------|-----|--------|-------------|
| 50 | 0,99 | 0,30 (30%)  | 0,04 (4%)|
| 75 | 1,49 | 0,45 (30%)  | 0,07 (4,5%)|
| 100| 1,99 | 0,60 (30%)  | 0,09 (4,5%)|
| 150| 2,99 | 0,90 (30%)  | 0,14 (4,6%)|
| 250| 4,99 | 1,50 (30%)  | 0,24 (4,8%)|
| 350| 6,99 | 2,10 (30%)  | 0,34 (4,8%)|
| 500| 9,99 | 3,00 (30%)  | 0,49 (4,9%)|
| 750|14,99 | 4,50 (30%)  | 0,74 (4,9%)|
|1000|19,99 | 6,00 (30%)  | 0,99 (5%)|
|1500|29,99 | 9,00 (30%)  | 1,49 (5%)|
|2500|49,99 | 15,00 (30%) | 2,49 (5%)|

Tabela com os preços de apps de lojas. [Fonte](https://core.telegram.org/bots/payments-stars#star-pricing).

#### Fragment 

> [Fragment.com](https://fragment.com/stars)

|Quantidade|Preço|Taxa Telegram|
|----|------|---|
| 50 | 0,75 | 0,10 (13,3%)|
| 75 | 1,13 | 0,15 (13,3%)|
|100 | 1,50 | 0,20 (13,3%)|
|150 | 2,25 | 0,30 (13,3%)|
|250 | 3,75 | 0,50 (13,3%)|
|350 | 5,25 | 0,70 (13,3%)|
|500 | 7,50 | 1,00 (13,3%)|
|750 | 11,25| 1,50 (13,3%)|
|1000| 15,00| 2,00 (13,3%)|
|1500| 22,50| 3,00 (13,3%)|
|2500| 37,50| 5,00 (13,3%)|

#### Direct

> [Site Telegram](https://telegram.org/apps)

|Quantidade|Preço|Taxa Telegram|
|----------|-----|-------------|
| 50 | 0,93 | 0,28 (30%) |
| 75 | 1,32 | 0,35 (26%) |
| 100| 1,69 | 0,39 (23%) |
| 150| 2,49 | 0,54 (22%) |
| 250| 4,03 | 0,78 (19%) |
| 350| 5,59 | 1,04 (19%) |
| 500| 7,89 | 1,39 (18%) |


* `Quantidade`: Quantidade de estrelas compradas
* `Preço`: Valor pago em dólares americanos
* `Comissão loja`: Comissão das lojas da Apple e do Google
* `Taxa Telegram`: Taxa cobrada pelo Telegram

## Saque

<b>O valor para saque é de US$ 0,013 cada estrela</b>[^saque]. Ou seja, independente de como foi feita a compra, o desenvolvedor do bot receberá 0,013 centavos de dólar em cada estrela. O requisito para o saque é ter pelo menos 1.000 estrelas há pelo menos 21 dias[^21dias].

## Consultar Saldo

O saldo é visualizado individualmente em cada chat, seja ele um bot ou um canal. Infelizmente os saldos não são agrupados. Ou seja, mesmo que sejam atingidas 998 estrelas em um bot e 500 em um canal, o saque não será permitido. É necessário que o requisito seja atingido no chat individualmente.

<img src="/assets/img/TelegramStarsSaldo.png" alt="Saldo de Stars em um bot." style="width:50%">
<br><small>Saldo de Stars em um bot.</small>

## Uso

Estou usando as estrelas em dois bots, [@Send2KindleBot](https://t.me/Send2KindleBot) e [@ClientesChatBot](https://t.me/ClientesChatBot). A solução foi implementada não por ser ótima, mas sim por ser uma imposição do app. Caso não tenha implementado as estrelas em seus bots, canais ou grupos, recomendo que o faça, caso contrário o Telegram pode punir sua solução, tornando seu chat indisponível.

## Conclusão

Apesar de ser uma solução de fácil implementação nos bots e canais, as Estrelas estão longe de ser uma solução ideal. Infelizmente, dentre os usuários de meus bots, o comentário mais comum foi: <i>Não encontrei onde comprar as estrelas</i>. Ou seja, há ainda um longo caminho de ensinamentos a ser trilhado. O Telegram não fez nenhuma ação que explicasse o funcionamento ou a aquisição das estrelas. Além disso, o fato de os saldos não serem agrupados me preocupa muito, pois é possível que as estrelas se percam antes do prazo possível para o saque.

> Stars earned in connection with the content you publish on Telegram are valid for 3 years from the date they were received. Should you fail to accept rewards with the Stars you accrued within their 3 year period of validity, they will be considered forfeit and debited from your balance[^3anos].

Ou seja, se no período de três anos não for feito o saque, o saldo será <i>incorporado</i> ao patrimônio do Telegram. 🤡

---

[^telegramblog]: [Post oficial do Telegram](https://telegram.org/blog/telegram-stars/pt-br?ln=a).

[^starstos]: [Termos de uso da moeda Telegram Stars](https://telegram.org/tos/stars?setln=pt-br).

[^saque]: [Termos de uso para Criadores de Conteúdo, seção Conteúdo Pago](https://telegram.org/tos/content-creator-rewards#2-2-paid-content)

[^21dias]: [Termos de uso para Criadores de Conteúdo, seção Saldo](https://telegram.org/tos/content-creator-rewards#4-1-balance)

[^3anos]: [Termos de uso para Criadores de Conteúdo, seção Validade](https://telegram.org/tos/content-creator-rewards#4-7-expiration)

