---
layout: post
title: "Como criar a Leitura Rápida no Telegram"
date: 2023-10-22 12:00:00 -0300
categories: [Telegram, Curiosidades]
tags: telegram instantview leiturarápida leitura telegraph
image: "https://instantview.telegram.org/file/811140067/3/6oS3A6aSzxU.261217/1d6f75b81ed67c983a"
---

# Leitura Rápida

O Telegram oferece uma poderosa ferramenta de visualizar conteúdo chamada Leitura Rápida. O conteúdo visto desta forma é rapidamente carregado e retém o usuário no aplicativo, mantendo a pessoa engajada em seu canal ou grupo. Além disso, o acesso não sobrecarrega seu servidor, tudo fica hospedado no próprio Telegram. É uma forma grátis de ter muita gente lendo o seu conteúdo simultaneamente.

Porém, alguns pontos podem não agradar a todos.

#### Não exibe anúncios

O conteúdo exibido em leitura rápida não exibe anúncios, ou seja, tem o potencial de afetar a renda do produtor de conteúdo.

#### Visita não conta em Analytics

Caso o produtor de conteúdo utilize alguma ferramenta de <i>Analytics</i>, as leituras feitas pela Leitura Rápida não serão contabilizadas como visitas uma vez que o site em si não recebe visita alguma.

#### Falta de concursos

O Telegram não realiza [concursos](https://instantview.telegram.org/contest/) de <i>templates</i> de Leitura Rápida desde 2019. A consequência disso é que não é mais possível criar um <i>template</i> público para um site, que funcione automaticamente ao compartilhar um link. A leitura rápida de novos sites somente é possível com soluções personalizadas, logo, se o link for compartilhado sem os devidos ajustes, a opção de leitura rápida não será exibida.

# Guia Rápido

Explico abaixo duas maneiras de criar a Leitura Rápida de qualquer conteúdo. A primeira permite maiores ajustes, porém, funciona exclusivamente em sites. Já a segunda, usando a api do Telegraph, permite a criação da Leitura Rápida de absolutamente qualquer conteúdo, mesmo que um site não exista.

## Leitura Rápida usando Templates

Acesse o site [instantview.telegram.org](https://instantview.telegram.org/) e autentique-se.

Clique em <i>My Templates</i> e cole a URL no campo que deseja criar o <i>template</i>.

<img src="/assets/img/InstantView.png" alt="Tela de criação de Template de Leitura Rápida."> 
<br><small>Tela de criação de template de Leitura Rápida.</small>

A tela de criação de um <i>template</i> é dividida em três janelas. A primeira é o site original, a do meio contém as regras de criação e a terceira traz a pré-visualização da Leitura Rápida.

### Formato das regras

* `title` (obrigatório): Título da postagem;
* `body` (obrigatório): Corpo da postagem;
* `subtitle`: Subtítulo;
* `kicker`: Primeira linha do template;
* `author`: Autor(a);
* `author_url`: Link do autor(a);
* `published_date`: Data da publicação em formado Unix;
* `description`: Descrição curta exibida na pré-visualização;
* `image_url`: Link da imagem exibida na pré-visualização;
* `document_url`: Link do documento exibido na pré-visualização;
* `site_name`: Nome do site exibido na pré-visualização;
* `channel`: Canal do autor no formato `@canal`;
* `cover`: Capa da página (imagem, vídeo, mapa).

#### Exemplo de regra

```xml
~version: "2.1"
title: $body//h1[1]
body: //*[contains(@class, "post-content")]
cover: //img[has-class("lazyloaded")]
@set_attr("src", @data-src): $body//img[@data-src]
channel: "@GabRF"
site_name: "blog.GabRF.com"
@split_parent: //a/img
@split_parent: //p/img
```

- O título será o conteúdo do primeiro cabeçalho do tipo `h1`;
- O corpo será o conteúdo da classe `post-content`;
- A capa será a imagem de classe `lazyloaded`;
- No corpo o parâmetro `src` (usado em imagens) será preenchido com o valor da tag `data-src`;
- Canal exibido no topo, ao lado do botão `Join`;
- Nome do site exibido na pré-visualização;
- Duas regras que tratam erros do tipo `Element <img> is not supported in <a>`[^elementnotsupported].

### Salvar e Utilizar

Após criar a regra do <i>template</i>, clique em `Mark as Checked` para o Telegram salvar as mudanças. Feito isto, clique em `View in Telegram` e compartilhe em suas <i>Mensagens Salvas</i>. A intenção neste passo é simplesmente o de copiar o link para obter o identificador de <i>template</i>. 

O link gerado terá o seguinte formato: `https://t.me/iv?url=<URL_DO_POST>%2F&rhash=<IDENTIFICADOR_DO_TEMPLATE>`, composto por:

* `URL_DO_POST`: Link direto para o conteúdo daquele domínio (em formato de URL);
* `IDENTIFICADOR_DO_TEMPLATE`: Valor fixo correspondente ao template criado.

No exemplo, o link gerado foi o `https://t.me/iv?url=https%3A%2F%2Fblog.gabrf.com%2Fposts%2FFimDoRastreioBot%2F&rhash=36c1a8c5edccc1`. Repita o teste em outros links do mesmo site para confirmar se o <i>template</i> realmente está bom. O ideal é testar ao menos 10 links diferentes.

> É importante saber que o identificador de <i>template</i> é fixo. Ou seja, é possível atualizar as regras a qualquer momento e isto irá afetar a Leitura Rápida de todo aquele site, tanto as futuras quanto as já enviadas. O valor de identificador é único, vinculado à sua conta do Telegram. Logo, se duas ou mais pessoas criarem regras para o mesmo site, cada pessoa terá o seu identificador.

#### Exemplo de função em python

```python
import urllib # pip install urllib

rhash = '36c1a8c5edccc1' # Identificador do Template

def get_iv_link(link):
    iv_link = f'https://t.me/iv?url={urllib.parse.quote_plus(link)}&rhash={rhash}'
    return iv_link
```

Esta função recebe a <i>string</i> `https://blog.gabrf.com/posts/FimDoRastreioBot/` e retorna o link com Leitura Rápida: `https://t.me/iv?url=https%3A%2F%2Fblog.gabrf.com%2Fposts%2FFimDoRastreioBot%2F&rhash=36c1a8c5edccc1`.

## Leitura Rápida via API Telegraph

O Telegram possui uma plataforma de publicação de textos chamada [Telegraph](https://telegra.ph/), que pode ser usada diretamente pelo [site](https://telegra.ph/) ou via [API pública](https://telegra.ph/api). Todo texto criado na plataforma automaticamente tem o recurso de Leitura Rápida habilitado, podendo, portanto, ser uma solução para quem não possui um site ou possui alguma dificuldade em utilizar [<i>templates</i>](#leitura-r%C3%A1pida-usando-templates), como explicado anteriormente[^geoblock].

### Autenticação

O primeiro passo é criar a sua chave de acesso. Para isto, acesse a url abaixo substituindo os valores `<NOME_CURTO>` e `<NOME>` pelo que julgar adequado:

```
https://api.telegra.ph/createAccount?short_name=<NOME_CURTO>&author_name=<NOME>
```

O resultado deverá ser parecido com este:

```yaml
{
  "ok":true,
  "result":{
    "short_name":"Sandbox",
    "author_name":"Anonymous",
    "author_url":"",
    "access_token":"d3b25feccb89e508a9114afb82aa421fe2a9712b963b387cc5ad71e58722",
    "auth_url":"https:\/\/edit.telegra.ph\/auth\/d3b25feccb89e508a9114afb82aa421fe2a9712b963b387cc5ad71e58722"
  }
}
```

O valor de `access_token` é o que será usado daqui em diante em uma variável de mesmo nome.

> Este valor não deve ser compartilhado com ninguém. Deve ser guardado como uma senha.

### Instalação da biblioteca em Python

Instale a biblioteca conforme o seu cenário de configuração. O método mais simples seria usando `pip install`.

```bash
$ python3 -m pip install telegraph
# Alternativamente, opção assíncrona
$ python3 -m pip install 'telegraph[aio]'
```

### Exemplo de uso da biblioteca em Python

```python
import telegraph

def create_telegraph_post(titulo, imagem, subtitulo, texto, link, autor):
    telegraph_auth = telegraph.Telegraph(
        access_token=access_token
    )
    response = telegraph_auth.create_page(
        f'{titulo}',
        html_content=(
            f'<img src="{imagem}">' +
            f'<h4>{subtitulo}</h4><br><br>' +
            f'{texto}<br><br>' +
            f'<a href="{link}">Leia a matéria original</a>'
        ),
        author_name=f'{autor}'
    )
    return data
```

A função `create_telegraph_post` recebe como parâmetros o título, a imagem, um subtítulo, o texto completo, o link para o site e o nome do autor respectivamente. A resposta será:

```yaml
{
  'path': 'Título-10-22-5',
  'url': 'https://telegra.ph/Título-10-22-5',
  'title': 'Título',
  'description': '',
  'author_name': 'Autor',
  'views': 0,
  'can_edit': True
}
```

Pronto. Agora basta compartilhar o link presente em `url` e a função de Leitura Rápida irá aparecer.

<img src="/assets/img/InstantViewChat.png" alt="Link do Telegraph com Leitura Rápida." style="width:50%">
<br><small>Link do Telegraph com Leitura Rápida.</small>

<img src="/assets/img/InstantViewResult.png" alt="Exemplo de texto visto em modo de Leitura Rápida." style="width:50%">
<br><small>Exemplo de texto visto em modo de Leitura Rápida.</small>

---

### Envio de imagens para o Telegraph

> Explicação bônus, método não oficial!

Há casos em que a exibição de imagem não funciona, pois o site que hospeda a imagem pode colocar algumas proteções que a impeçam de serem compartilhadas em sites terceiros. A solução para tal caso seria o de enviar a imagem para o Telegraph, porém, este método não está oficialmente na API. <b>Use-o sabendo que é uma gambiarra</b>!

```python
import io
import requests
import telegraph

def upload_telegraph_image(image):
    telegraph_auth = telegraph.Telegraph(
        access_token=access_token
    )
    file = requests.get(image)
    inmemoryfile = io.BytesIO(file.content)
    path = telegraph_auth.upload_file(inmemoryfile)
    return f'https://telegra.ph{path[0]["src"]}'
```

A função `upload_telegraph_image` fará o upload da imagem contida na URL para o Telegragh, criando um novo endereço para visualização, exemplo:

```python
if __name__ == "__main__":
    data = upload_telegraph_image('https://instantview.telegram.org/file/811140067/3/6oS3A6aSzxU.261217/1d6f75b81ed67c983a')
    print(data)
```

Cuja resposta será:

```
https://telegra.ph/file/2abe611003a6c9aa2f5f4.png
```

Que é a imagem oficial do Telegram usando no [exemplo acima](#exemplo-de-uso-da-biblioteca-em-python).

---

[^elementnotsupported]: Erro muito comum. Acontece quando elementos estão aninhados e não são suportados pela Leitura Rápida. A regra `split_parent` retira esse aninhamento, resolvendo o problema. [Leia mais no site oficial](https://instantview.telegram.org/docs#split-parent).

[^geoblock]: Há casos, por exemplo, em que um site não pode ser acessado de determinados países, impedindo a solução que usa <i>templates</i> de funcionar. Usando a opção via Telegraph é possível capturar os dados do site em seu próprio computador, evitando assim o problema de bloqueio por geo-localização.
