#attack #attack/web #web

**SSRF** (Server-Side Request Forgery) o invasor pode fornecer sua própria entrada na forma de uma URL para acessar recursos remotos recuperados pelo servidor.

## Tipos de SSRF:

**In-Band**: a requisição é enviada e o conteúdo do URL fornecido é exibido na resposta

| Req | Res |
| --- | --- |
| POST api/v1/store/products<br>...<br><br>{<br>"inventory":"http://localhost.com/secrets"<br>} | HTTP/1.1 200 OK<br>...<br><br>{<br>"secret_token":"QdwdxurLd3"<br>} |

**Blind:** a requisição é enviada, o servidor executa porém não retorna na resposta o resultado do ataque

| Req | Res |
| --- | --- |
| POST api/v1/store/products<br>...<br><br>{<br>"inventory":"http://localhost.com/secrets"<br>} | HTTP/1.1 200 OK<br>...<br><br>{} |
Por mais que o servidor não retorna um conteúdo esperado, há formas de analisar se o ataque foi um sucesso com o **Burp Suite Collaborator** (que só tem no Burp Pro :c) ou com alternativas gratuitas como:
- http://webhook.site/
- https://requestbin.com/
- https://canarytokens.org/
- Ngrok

## Por onde procurar procurar

- URLs parciais ou completas no corpo ou nos parâmetro de POST
- Headers que inclui URLs como *Referer*
- Entrada do usuário que pode resultar em um servidor recuperando recursos
- [SSRFMap](https://github.com/swisskyrepo/SSRFmap)

## Bypass

#### Filtro no input baseado em blacklist

Algumas aplicações criam uma lista com *hostnames* como `127.0.0.1` e `localhost` ou URLs sensíveis com `/admin` para se defender contra SSRF. Nestes casos algumas das formas de contornar são:

- Usar IP alternativo. Ex: `127.0.0.1` -> `2130706433`, `017700000001`, `127.1`
- Utilizar o próprio domínio com o **webhook.site** ou **ngrok**
- Obfuscar strings que são bloqueadas com URL encode ou *case variation*

#### Filtro no input baseado em whitelist

Algumas aplicações só permite determinado input que da *match* com os valores de uma whitelist. Para burlar isso precisa entender como a URL é formada e achar inconsistências.

- Adicionar credenciais na URL depois do hostname usando @
	`https://expected-host:fakepassword@evil-host`
* Fragmentar URL com # 
	`https://evil-host#expected-host`
- Colocar um domínio que você controla aproveitando a hierarquia de DNS
	`https://expected-host.evil-host`
- Obfuscar strings que são bloqueadas com URL encode
- Combinar tudo

Lab: [[SSRF - whitelist-based input filter]]

## Referência

https://portswigger.net/web-security/ssrf#what-is-ssrf