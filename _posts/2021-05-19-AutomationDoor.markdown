---
layout: post
title: "Automação residencial: Fechadura digital e Controle de Acesso"
date: 2021-05-19 17:00:00 -0300
categories: [Automação Residencial, Fechadura digital e Controle de acesso]
tags: automação fechadura controlid digital senha nfc
image: "/assets/img/automationimg.png"
---

> Acesso autorizado.

Primeiro ítem em uma automação residencial, o controle de acesso é talvez a escolha mais complexa e que envolve analisar mais parâmetros que qualquer outra solução.

De início é importante separar o que é fechadura do que é o equipamento de controle de acesso. Há modelos que são soluções completas, mas este nem sempre é o caso.

## Fechadura Digital

Pode ser entendida como uma solução completa. É comprado um kit completo, pronto para ser instalado. Possui todos os componentes, desde a leitora de biometria/teclado númerico até o trinco que tranca efetivamente a porta.

São alimentadas por pilhas e são de fácil instalação. Raros modelos se conectam à rede sem fio ou ao celular usando bluetooth. O mais comum é que não possuam conectividade, preservando as pilhas.

Em caso de as baterias acabarem, possuem contatos externos em que basta encostar uma bateria 9V que a fechadura funcionará para que permita um acionamento, abrindo a porta.

<center>
<img src="/assets/img/controlid_idlockbio.png" alt="ControlID IDLock Bio. Parte externa (esq) e parte interna." style="width:50%"> 
<br><small>ControlID IDLock Bio. Parte externa (esq) e parte interna.</small>
</center>

## Controle de Acesso

Soluções de controle de acesso têm como principal diferencial que as partes devem ser adquiridas separadamente. É possível, inclusive, misturar marcas. Ou seja, é necessário escolher qual será o controle de acesso e também qual será a fechadura eletrônica que serão usados.

### Leitor biométrico

Controle de acesso usando-se dados biométricos. Geralmente é feita a leitura da impressão digital, mas há modelos que conseguem verificar a face ou até mesmo a irís. São soluções muito seguras e que não permitem compartilhamento.

<center>
<img src="/assets/img/controlid_idflexpro.png" alt="ControlID IDFlex Pro." style="width:50%"> 
<br><small>ControlID IDFlex Pro.</small>
</center>

### Senha

O modelo mais comum. A pessoa digita uma senha em um teclado e a porta será destravada.

<center>
<img src="/assets/img/intelbras_fr210.png" alt="Intelbras FR210." style="width:20%"> 
<br><small>Intelbras FR210.</small>
</center>

### Tag NFC

Etiquetas de aproximação. Podem ser em formato de cartão, chaveiro, coleira, etiqueta etc. Basta aproximar da leitora e o acesso será permitido. 

<center>
<img src="/assets/img/nfc_reader.png" alt="Leitor de cartão por aproximação." style="width:50%"> 
<br><small>Leitor de cartão por aproximação.</small>
</center>

## Fechaduras

### Solenóide

Fechadura em que um pino controla a abertura e o fechamento. Há modelos que permitem uso da chave, muito útil em situações de falta de energia, por exemplo. Geralmente possuem sensor de porta aberta embutido. Normalmente usada em portas de madeira, pois comportam a fechadura. Não é possível usar em portas finas, como portas de vidro.

Os modelos variam em:

`Fail safe`: Pino recolhido quando há falta de energia elétrica, ou seja, a porta fica aberta.

`Fail secure`: Pino fica extendido se não houver eletricidade, mantendo a porta trancada.

<center>
<img src="/assets/img/fechadura_solenoide.png" alt="Fechadura solenóide." style="width:20%"> 
<br><small>Fechadura solenóide.</small>
</center>

### Strike

Modelo em que é enviado um pulso e o trinco pode ser recolhido, permitindo a abertura. Geralmente possui uma maçaneta para o lado de dentro. Pode ser sobreposta ou interna à porta. Muito usada em portas de vidro. São do tipo `fail secure`, ou seja, não permanecem abertas caso falte energia elétrica.

<center>
<img src="/assets/img/fechadura_strike.png" alt="Fechadura strike Amelco." style="width:50%"> 
<br><small>Fechadura strike Amelco.</small>
</center>

### Eletroimã

Geralmente usada em portas de vidro, as fechaduras do tipo eletroimã abrem quando a energia é desligada, ou seja, são do tipo `fail safe`. Portanto, recomenda-se o uso de baterias ou outros sistemas de contingência para casos de falha na rede elétrica. Podem ser instaladas basicamente em qualquer porta por serem de sobrepor.

> É importante observar no ato da compra se a versão adquirida é para portas que abrem para dentro ou para fora.

<center>
<img src="/assets/img/fechadura_eletroima.png" alt="Fechadura eletroimã Intelbras." style="width:30%"> 
<br><small>Fechadura eletroimã Intelbras.</small>
</center>

## Conclusão e Recomendação

Gosto muito da marca ControlID. São produtos nacionais, funcionam muito bem e possuem inúmeras funcionalidades já embutidas nos produtos, além de um suporte muito bom. São facilmente encontradas no Mercado Livre e têm ótima relação custo/benefício.

Costumo recomendar dois modelos: A ControlID IDFlex Pro e a ControlID IDLock Bio.

### ControlID IDFlex Pro

> Modelo que não é o Pro não possui rede! Perde muita funcionalidade.

Modelo que uso onde moro, em conjunto com uma fechadura solenóide (Modelo: Automatiza FS1010). Destaco que este modelo se conecta à rede (LAN), possui API e permite a abertura usando chave (de acordo com a fechadura usada).

- LAN: Permite acesso do computador ou do celular pela rede. A interface web é ótima. Possui relatórios de acessos, customização do período de acesso individualizado e personalização completa do formulário das pessoas, permitindo inserir foto, nome, cpf, telefone etc. As possibilidades são infinitas.

- API: Para os desenvolvedores, a API é um enorme diferencial para criar integrações e soluções novas. As possibilidades são infitias. Uso, por exemplo, para receber uma mensagem quando alguém abre a porta.

De ponto negativo, não há como fugir da dificuldade de instalação. É necessário chegar com rede elétrica na fechadura e na leitora de digital.

[Site do fabricante](https://www.controlid.com.br/controle-de-acesso/idflex/).

### ControlID IDLock Bio

De facílima instalação, a IDLock Bio é ideal para quem quer ter pouquíssimo trabalho e quer usar uma solução completa. Funciona com pilhas AA e a expectativa é que as pilhas durem em torno de um ano.

Possui todas as personalizações de cadastro da IDFlex, com o diferencial de não ser acessada pela rede. Sua conexão é feita via Bluetooth, permitindo fazer tudo por aplicativo de celular. O aplicativo é sincronizado com a fechadura e permite a emissão de senhas mesmo quando o telefone não está ao alcance do sinal Bluetooth. Por não estar ligada à rede, não é possível inserir novos cadastros quando se está longe da fechadura. Não possui API.

[Site do fabricante](https://www.controlid.com.br/controle-de-acesso/idlock-bio/).
