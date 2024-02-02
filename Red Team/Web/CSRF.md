 #web #vuln

**Cross-site request forgery** (CSRF) é uma vulnerabilidade web que permite o atacante induzir a vítima a fazer uma ação não intencional, como por exemplo alterar dados da conta, o que pode dar ao atacante acesso a esta conta.

O ataque acontece quando o atacante envia um website que contém um HTML malicioso para vítima, semelhante a exploração do ataque *reflected* [[XSS]].

![[Pasted image 20231218150350.png]]
## Onde procurar

* Ações relevantes como por exemplo, alterar dados da conta
* Carrinho de e-commerce

É importante que a requisição tenha *cookies*.

## Como mitigar

- CSRF tokens
- SameSite cookies
- Referer-based

## Burlando CSRF Token

O **CSRF Token** é uma chave única gerada pelo servidor e compartilhada com cliente, que ao fazer uma requisição, deve anexar essa chave no *header* ou no *body*.

* **Teste 1**: Trocar os métodos (Ex: POST -> GET)
* **Teste 2**: Omitir o *token*
* **Teste 3:** Usar *token* de outro usuário
* **Teste 4:** Testar se o *token* está ligado com o *cookie* de sessão (quando tem um token no *body* e no *cookie*)
	* Há casos onde o token do *body* e do *cookie* são duplicados, um possível teste é gerar um *token* inválido

## Burlando SameSite cookie

Esse mecanismo define quando um *cookie* é adicionado em requisições de *sites* distintos.
Neste contexto, um site é definido como TLD + níveis do domínio + scheme. Ex:

`https://admin.example.com`

- TLD -> .com
- +1 -> example
- scheme -> https

Diferente do *origin* (definido em [[SOP]]) um site consegue englobar vários subdomínios. Ex:

| Request from            | Request to                  | Same-site?               | Same-origin?                    |
|-------------------------|-----------------------------|--------------------------|---------------------------------|
| https://example.com      | https://example.com         | Sim                      | Sim                             |
| https://app.example.com  | https://intranet.example.com | Sim                      | Não: domínio diferente       |
| https://example.com      | https://example.com:8080     | Sim                      | Não: porta diferente             |
| https://example.com      | https://example.co.uk        | Não: TLD diferente       | Não: domínio diferente       |

O SameSite possui três tipos:
- **Strict:** os sites devem ser iguais se não o *cookie* não é incluido na requisição
- **Lax:** só funciona em métodos GET (pois CSRF é mais comum em métodos POST)
- **None:** Desativa as restrições SameSite mas tem que enviar o atributo *Secure* garantindo que os *cookies* só são enviados em mensagens criptografadas pelo HTTPS

### SameSite Lax

- **Teste 1:** Incluir o método POST -> [[CSRF - bypass SameSite Lax]]

## Burlando Referer header

O **Referer** é um parâmetro opcional enviado no cabeçalho da requisição contendo a URL do *Host*. É uma abordagem menos efetiva que geralmente é adicionada automaticamente pelo *browser*.

- **Teste 1:** Omitir o *Referer*
- **Teste 2:** Verificar qual parte do *referer* a aplicação valida. Ex:

```
GET /change-password HTTP/1.1 
Host: vulnerable-website.com              
Referer: http://hacked-domain.com/?vulnerable-website.com               
Cookie: session=...                          
```

O servidor pode verificar a parte válida (vulnerable-website.com) está contida no *referer*.

## Referência
https://portswigger.net/web-security/csrf
https://book.hacktricks.xyz/pentesting-web/csrf-cross-site-request-forgery