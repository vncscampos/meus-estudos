https://portswigger.net/web-security/jwt/algorithm-confusion/lab-jwt-authentication-bypass-via-algorithm-confusion

1. Para conseguir a chave pública bastar ir em `/jwks.json`
2. Criar uma nova chave RSA passando o objeto acima em `JWT Editor`
3. Botão direto na chave criada -> "Copy public key as PEM"
4. Usar **Decoder** para codificar a key em base64
5. Volta no `JWT Editor`para gerar uma nova chave simétrica alterando o parêmetro "k" para o base64 acima
6. Na aba `JSON Web Token` da requisição alterar os payloads
7. Assinar alterando o header `alg`
8. GG