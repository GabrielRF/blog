---
layout: post
title: "Telegram não aumenta engajamento"
date: 2021-05-02 14:00:00 -0300
categories: [Telegram, Curiosidades]
tags: telegram ferramenta influencers engajamento analytics 
image: "/assets/img/TelegramTool.jpeg"
---

> Quem vê Analytics, não vê Telegram.

<center>
<img src="/assets/img/TelegramTool.jpeg" alt="Telegram como ferramenta. Imagem do Telegram." style="width:100%"> 
<br><small>Telegram como ferramenta. <a href="https://telegram.org/blog/profile-videos-people-nearby-and-more/pt-br">Imagem do Telegram</a>.</small>
</center>

Telegram é uma ótima ferramenta para engajar o público e manter toda a comunicação organizada. **Permite notificar 100% de seus seguidores instantaneamente e formar uma comunidade unida e forte**. Tudo sem exibir números de telefone e preservando a privacidade de todos.

## Canais

<center>
<img src="/assets/img/tginf_channel.png" alt="Canal @BsbDF" style="width:50%"> 
<br><small>Canal <a href="https://t.me/BsbDF">@BsbDF</a>.</small>
</center>

É a maneira mais fácil de distribuir seu conteúdo. Canais são locais em que somente a pessoa que criou (e quem mais for autorizado) pode escrever. Os posts são entregues instantaneamente para todos os assinantes. **A lista de pessoas é restrita aos administradores**.

Os canais podem ser públicos, sendo acessíveis com um <code>@nome</code> e mantendo o histórico visível para todos. **Podem também ser vinculados a grupos, permitindo assim que as pessoas façam comentários nas postagens** e interajam entre si e também com os administradores. A vinculação é feita nas propriedades do canal. Somente a pessoa que for dona do canal e do grupo pode fazer a integração.

<center>
<img src="/assets/img/tginf_vincular.png" alt="Vincular canal a grupo" style="width:50%"> 
<br><small>Vincular canal a grupo.</small>
</center>

## Grupos

Assim como canais, grupos também podem ser públicos. A grande diferença de um grupo para um canal é que no segundo todos podem falar e podem ver a lista de membros. **Os administradores podem ser colocados como anônimos** pelo dono do grupo. Quando anônimos, não aparecerão na listagem de pessoas e suas mensagens sairão em nome do grupo.

<center>
<img src="/assets/img/tginf_anonymous.png" alt="Opção de permanecer anônimo" style="width:50%"> 
<br><small>Opção de permanecer anônimo.</small>
</center>

O grupo, quando vinculado ao canal, pode ser acessado nas propriedades do canal, clicando-se no botão <code>Conversar</code>. Caso haja interesse que os membros do canal somente comentem no canal e não no grupo[^1], mantendo a organização, bots podem ser usados para não permitir a entrada de pessoas no grupo. O [@AutoKick_bot](https://t.me/autokick_bot) é um exemplo de bot que pode ser colocado no **grupo como administrador** para garantir que as pessoas usem somente o canal.

<center>
<img src="/assets/img/tginf_conversar.png" alt="Opção conversar em um canal" style="width:50%"> 
<br><small>Opção conversar em um canal.</small>
</center>

## Analytics

<center>
<img src="/assets/img/tginf_twitterclick.png" alt="Analytics: Clique com origem Twitter" style="width:100%"> 
<br><small>Analytics: Clique com origem Twitter.</small>
</center>

Diferentemente do Twitter, Facebook e outras redes, **o Telegram não altera os links**. Por isto, não é possível saber que o clique em seu site ou vídeo veio do app. Os cliques são considerados cliques `diretos`, não tendo uma origem definida.

<center>
<img src="/assets/img/tginf_direct.png" alt="Analytics: Clique direto." style="width:100%"> 
<br><small>Analytics: Clique direto.</small>
</center>

Ou seja, a menos que os parâmetros `UTM`[^2] sejam inseridos no link, **as ferramentas de Analytics não saberão a origem do clique**, não deixando claro o que veio do Telegram e o que não veio. 

Exemplo de caso de uso: Em vez de usar no Telegram o link `https://gabrf.com`, pode-se usar o link `https://blog.gabrf.com/?utm_source=telegram&utm_medium=telegrammessage&utm_campaign=rastreiobot`. Desta forma todos os parâmetros estão definidos e os cliques contados corretamente. 

<center>
<img src="/assets/img/tginf_telegramclick.png" alt="Analytics: Clique com origem Telegram" style="width:100%"> 
<br><small>Analytics: Clique com origem Telegram.</small>
</center>

## Conclusão

**O Telegram pode sim ser uma excelente ferramenta para engajar as pessoas.** Porém, é importante saber que o app, por ter um enorme compromisso com a privacidade, acaba por não entregar a origem dos cliques, dando a impressão que o app não gera cliques ou engajamento.

Caso a contagem de cliques seja relevante, é importante conhecer as regras do jogo e as ferramentas disponíveis. **Um simples ajuste nos links fará toda a diferença na contagem**, sejam os links para sites, vídeos do YouTube ou qualquer outra rede.

---

[^1]: Após a vinculação, tudo o que é postado no canal aparece no grupo, mas o que é postado no grupo não aparece no canal!

[^2]: Ferramenta para definição de parâmetros UTM: [https://ga-dev-tools.appspot.com/campaign-url-builder/](https://ga-dev-tools.appspot.com/campaign-url-builder/)
