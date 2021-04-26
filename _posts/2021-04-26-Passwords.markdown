---
layout: post
title:  "Sua senha deve conter:"
date:   2021-04-26 10:30:00 -0300
categories: [Segurança, Gerenciadores de senhas]
tags: senhas segurança gerenciador cofre
image: "https://blog.gabrf.com/assets/img/enpass_control.jpg"
---
- Letras maiúsculas
- Letras minúsculas
- Números
- Caracteres especiais
- Gifs
- Mais números ainda
- Mais umas letras

---

### Calma...

Muito se fala em não repetir as senhas, em ter senhas longas, em usar números, letras e caracteres especiais para se criar uma senha segura. Mas ninguém explica como fazer isto de maneira prática e que não exija um monte de papel guardado na gaveta com as senhas.

## Gerenciadores de senhas 

São aplicativos que armazenam suas credenciais (logins, senhas etc) de maneira segura, **com criptografia**, em um arquivo chamado **cofre**. Podem ser integrados aos navegadores e aos aparelhos celulares, tornando possível o uso de senhas bastante complicadas sem que isto se torne muito trabalhoso. Basta cadastrar a senha no app e deixar o navegador auto-completar para você. Em aparelhos com reconhecimento biométrico, as senhas são preenchidas usando-se tais mecanismos, ou seja, tudo muito fácil e cômodo.

<center>
<img src="/assets/img/enpass_usage.gif" alt="Auto-preenchimento de senha" style="width:50%; border-radius:50%"> 
<br><small>Auto-preenchimento de senha</small>
</center>

No momento em que é criado um novo cadastro o app sugere uma senha, que é salva automaticamente salva no cofre. Ou seja, não é trabalhoso para usar a senha nem para criar um novo cadastro.

<center>
<img src="/assets/img/enpass_control.jpg" alt="EnPass: Tela de controle" style="width:50%"> 
<br><small>EnPass: Tela de controle</small>
</center>

Além de armazenar as credenciais, muitos gerenciadores de senhas verificam se você foi vítima de vazamentos. São calculados os *hashes* das senhas e são comparados com dados disponíveis online. Ou seja, a verificação é feita de maneira segura, pois ninguém vê a sua senha, nem mesmo o app. Assim, fico sabendo de problemas sem que a autoridade do banco de dados precise me notificar, pois é comum que tais avisos sejam feitos com considerável atraso.

## Qual utilizar?

Recomendo gerenciadores que não usem nuvem própria, ou seja, o cofre fica armazenado onde você escolher. Desta forma, supondo que seja descoberta uma falha de segurança no gerenciador, será necessário também invadir o serviço de armazenamento (Dropbox, Drive etc) para que se tenha acesso ao cofre.

Gerenciadores de senhas que eu recomendo (em ordem alfabética):

### [1Password](https://1password.com/pt/)
Muito famoso entre usuários de dispositivos Apple.
Ponto fraco: Uso limitado fora do ambiente Apple, motivo que me fez migrar.

### [BitWarden](https://bitwarden.com/)
Gerenciador totalmente de código aberto. Permite que qualquer pessoa avalie como o programa funciona e faça uma auditoria de segurança.
Aplicativo gratuito para uso individual. Nunca o utilizei, mas tem uma fama excelente.

### [Enpass](https://www.enpass.io/)
É o que uso atualmente. O que me atraiu na época foi o funcionamento nos principais sistemas operacionais do Mercado. Funciona muitíssimo bem. 

### [LessPass](https://lesspass.com/)
Gerenciador de senhas open source e que não possui cofre. A senha é calculada de acordo com parâmetros de cada serviço/site no momento do uso. É um paradigma novo comparado aos anteriores.

## Na prática

Não vivo sem um gerenciador de senhas. Sempre que o sistema permite, coloco senhas com mais de 50 caracteres. Ainda, com o uso do [MailShieldBot](https://mailshield.xyz), além de uma senha por serviço, tenho também um e-mail por serviço, tornando impossível lembrar de todas as informações sem um cofre/gerenciador de senhas. Tenho também o hábito de salvar os dados de meus documentos em notas no cofre, tendo sempre em local de fácil acesso números que nunca me recordo, como PIS, número do título de eleitor ou de reservista. 

Neste cenário, mesmo que haja um vazamento de dados, que o app me notifica, sei que o prejuízo máximo não será muito grande, pois a senha vazada funciona em no máximo um local e o e-mail vazado é exclusivo do serviço, ou seja, a pessoa que obteve os dados pouco conseguirá fazer, pois não será possível autenticar em nenhum outro local. Nem a senha nem o e-mail serão válidos em outros sites e serviços. O quanto antes providenciarei a troca da senha vazada e pronto. Não preciso mexer em vários locais para manter meus dados seguros. 

E assim, durmo tranquilo!

<center><b>Use autenticação em dois fatores!</b></center>
