#ctf

https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal

1. Gerar um key simétrica com `JWT Key Editor`
2. Trocar o "k" da chave para byte nulo codificado em Base64 `AA==`
3. Alterar o kid na aba `JSON Web Token` da requisição -> `../../../../../../../dev/null`
4. Assinar com chave simétrica criada sem alterar header