#techniques #techniques/api #techniques/web 

**Fuzzing** é uma técnica de enviar entradas inesperadas para o servidor, para ver como ele vai reagir e descobrir coisas, como um vulnerabilidade, arquivos etc.

A diferença entre *bruteforce* e *fuzzing* é que no ataque de força bruta, são enviados dados válidos para tentar passar de uma validação da aplicação. Já no *fuzzing* é possível enviar dados aleatórios para tentar quebrar essa validação.

## Enum APIs

```bash
ffuf -u https://$IP/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt

ffuf -u https://$IP/api/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-small-words.txt
```

## Enum subdomínios

```bash
ffuf -u http://FUZZ.$IP/ -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Enum extensões

```sh
ffuf -u http://$IP/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-small-words.txt -e .php, .txt
```

## Múltiplos locais

```sh
ffuf -u https://W2/W1 -w ./wordlist.txt:W1,./domains.txt:W2
```

## Recursivo

```sh
ffuf -c -w /path/to/wordlist -u https://ffuf.io.fi/FUZZ -recursion -recursion-depth 2
```

## Filtros

```bash
ffuf -c -w /path/to/wordlist -u https://ffuf.io.fi -mc 200,301 #apenas 200,301

ffuf -c -w /path/to/wordlist -u https://ffuf.io.fi -ms 350 #tamanho da resposta

```

## Limite de requisição

Para testar em servidores que estão em produção ou bug bounty é útil limitar os números de requisição para não interromper o serviço ou deixa-lo lento.

```sh
ffuf -c -w /path/to/wordlist -u https://ffuf.io.fi -p 0.1 #delay entre req
ffuf -c -w /path/to/wordlist -u https://ffuf.io.fi -rate 10 #nº máximo de req
```

## Auto calibração

Útil para remover falsos positivos

```sh
ffuf -c -w /path/to/wordlist -u https://ffuf.io.fi -ac
```

## Referência

https://cybersecnerds.com/ffuf-everything-you-need-to-know/