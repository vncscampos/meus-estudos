#web #vuln

**Cross-site request forgery** (CSRF) é uma vulnerabilidade web que permite o atacante induzir a vítima a fazer uma ação não intencional, como por exemplo alterar dados da conta, o que pode dar ao atacante acesso a esta conta.

O ataque acontece quando o atacante envia um website que contém um HTML malicioso para vítima, semelhante a exploração do ataque *reflected* [[XSS]].

![[Pasted image 20231218150350.png]]
## Onde procurar
* Forms de ação de conta
* Carrinho de e-commerce

## Como mitigar

- CSRF tokens
- SameSite cookies

## Bypass

### CSRF Token



## Referência
https://portswigger.net/web-security/csrf
https://book.hacktricks.xyz/pentesting-web/csrf-cross-site-request-forgery