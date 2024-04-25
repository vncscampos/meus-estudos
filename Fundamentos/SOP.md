#web #fundamentals 

O SOP é um mecanismo de segurança do navegador que previne que sites atacam outros sites. Ele restringe que scripts de uma origem (*origin*) acessem dados de outra origem, ou seja, apenas as mesmas origens (*same-origin*) podem se comunicar.

O *origin* é uma URL que consiste em três partes: *Scheme*; *Domain* e *Port*

Por exemplo:
`http://normal-website.com/example/example.html` 

- Schema -> http
- Domain -> normal-website.com
- Port -> 80 (http)

Ou seja, uma URL que possua essas partes diferente por exemplo, se fosse https ou "normal-website2", o acesso não ia ser permitido.

O SOP geralmente controla o acesso que códigos JS têm ao conteúdo carregado de outros domínios (*cross-domain*). Conteúdos externos como imagens ou videos e até mesmo *scripts* podem ser incorporados na página, porém qualquer JS presente na página não vai conseguir ler o conteúdo desses recursos. Por exemplo, quando existem subdomínios, por mais que sejam origens diferente, a tag **HttpOnly** é uma medida de segurança para os cookies, ao restringir que scripts do lado do cliente, como JS,  tenha acesso ao cookie.
### Por que é importante

* Proteção contra [[XSS]]
* Proteção contra [[CSRF]]
* Controle de Acesso a recursos
* Prevenção de roubo de cookies

## Referência
https://portswigger.net/web-security/cors/same-origin-policy