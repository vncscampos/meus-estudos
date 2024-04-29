#attack #attack/web #web

## JWT Editor

Esse plugin do Burp permite ver o token jwt da requisição além criar assinatura na aba de (JWT Editor) e realizar ataques.

![[Pasted image 20240116135541.png]]

## Vulnerabilidades

### Quando a assinatura não é verificada

A biblioteca do `jsonwebtoken` possui uma função chamado `verify()`, no NodeJS por exemplo,  que se não for usada, é possível editar o header ou payload sem problemas. Utilizando o JWT Editor:
1. Editar os campos do payload
2. Clicar em **Sign**. Caso não tenha criado nenhuma assinatura, base ir na aba JWT Editor e escolher o método (New RSA Key, New Symmetric Key etc)
3. Enviar requisição

### Assinatura vazia

O header do JWT contém o parâmetro `alg` que informa ao servidor qual algoritmo foi usado para assinar o token, e portanto, qual algoritmo ele precisa usar ao verificar a assinatura. Como token é algo editável, é possível alterar o `alg:none` e assim parte da assinatura vai ficar vazia.

### Injeção de parâmetros no header JWT

No header do JWT existem três parâmetros opcionais que são interessantes:
- **jwk (json web key)** - Fornece um objeto JSON incorporado representando a chave.
- **jku (json web key set URL)** - Fornece uma URL da qual os servidores podem buscar um conjunto de chaves contendo a chave correta.
- **kid (Key ID)** - Fornece um ID que os servidores podem usar para identificar a chave correta em casos em que há várias chaves para escolher.

#### Via jwk

O jwk é um parâmetro de header opcional que os servidores podem usar para incorporar sua chave pública. Para fazer a verificação de assinaturas, servidores mal configurados às vezes utilizam qualquer chave dentro do jwk (o correto é usar uma lista restrita de chaves públicas). Com isso é possível injetar nossa própria chave privada RSA na chave pública do jwk.

1. Gerar uma nova chave RSA com **JWT Editor Keys**
2. Modificar payload
3. Clicar em **Attack** -> **Embedded JWK** -> Selecionar a chave criada no passo 1
4. Enviar requisição

#### Via jku

Em vez de incorporar as chaves públicas pelo jwk, um outra prática é usar o jku que diz onde acessar as chaves públicas. Explorar esse parâmetro consiste em alterar a url dele para url que contém a sua chave privada.

Lab: [[JWT - bypass jku header]]

#### Via kid

Alguns servidores armazenam várias chaves públicas e para identificar qual será usada é necessário o parâmetro `kid`. Esse parâmetro não possui uma especificação definida pelo JWS para sua estrutura, ou seja, fica a critério do desenvolvedor. Com isso, a `kid`pode ser um valor arbitrário ou até mesmo o caminho para um arquivo presente no servidor. Se esse parâmetro for vulnerável à [[Directory Transversal e LFI]] é possível forçar o fraudar o token.

Lab: [[JWT - bypass kid]]

### Ataques de confusão de algoritmo

Existem dois tipos de algoritmo usado no token:
- **Simétrico**: utiliza apenas uma chave, é o caso do HS256 (HMAC + SHA-256).
- **Assimétrico:** utiliza uma chave privada para assinar o token quando criado, e uma chave pública para verificar a assinatura, é o caso do RS256 (RSA + SHA-256).

Esse ataque ocorre quando o dev assume que vai sempre vim token feito com algoritmo configurado, por exemplo, o token foi assinado com base no algoritmo RS256 mas o servidor recebe um token simétrico como HS256, e assim a chave pública passada vai ser tratada como uma *secret*.

Para testar isso basta seguir os seguintes passos:
1. Obter chave pública do servidor
2. Converter a chave para formato correto
3. Criar um JWT modificado
4. Assinar o JWT usando o HS256 com a chave RSA pública

Lab: [[JWT - algorithm confusion]]

Quando a chave pública não está fácil de conseguir, uma forma é derivar a chave pública a partir de tokens JWT com ferramentas como:
- [rsa_sign2n](https://github.com/silentsignal/rsa_sign2n)
- jwt_forgery.py
- `docker run --rm -it portswigger/sig2n <token1> <token2>`
## Referência

https://portswigger.net/web-security/jwt/