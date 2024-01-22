#api
## Clássico

O ataque clássico inclui realizar uma força bruta para descobrir as senhas, com ferramentas como:

- Burp Intruder
- Wfuzz
- THC Hydra

É importante conhecer a política de senha antes de usar qualquer wordlist. Conhecendo essa política é possível usar comandos como:

```sh
cewl dominio.com.br -m 7 -w wordlist 
crunch 10 10 -t vinicius^%% -o wordlist
## ou mutar uma wordlist
john --wordlist=wordlist.txt --rules --stdout > wordlist_mutated
```

OBS: No diretório /etc/john/john.conf é possível adicionar novas regras para essa mutação.

## Análise de token

Alguns tokens podem ter uma padrão ao ser gerado. O **Sequencer** do Burp permite fazer uma análise de um token,  basta apenas configurar a localização do token e rodar.

![[Pasted image 20240116111726.png]]

Um token previsível teria este resultado no Sequencer. No exemplo abaixo, os 8 caracteres permanecem iguais e os 3 últimos se alteram. E assim, basta fazer uma requisição onde usa o token para cada combinação e ver qual da certo.

![[Pasted image 20240116112200.png]]

## JWT

O JWT é composto por três partes:
- **header** -> informação do algoritmo e do tipo do token
- **payload** -> informação do usuário
- **assinatura** -> validação do token

A assinatura utiliza uma chave secreta que valida todo token, ou seja, mesmo que o payload seja alterado a assinatura não vai corresponder e o token fica invalido. Porém conhecendo a assinatura é possível alterar o payload.

![[Pasted image 20240116113515.png]]

Mais testes para token jwt em: [[JWT]]