
https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection

1. criar uma RSA Key com o `JWT Key Editor`
2. colar a key no exploit server
```json
{
	"keys": [
		...chave gerada
	]
}
```
3. no header do jwt token:
	1. alterar o kid para nova kid criada da primeira etapa
	2. adicionar parâmetro `jku` com url do exploit server
4. Alterar o payload
5. Clicar em **Sign** (opção "Don't modify header" marcada)
6. GG