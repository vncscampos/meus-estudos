#recon 

**Objetivo:** Aumentar a superfície de ataque
## Passivo

Sem contato direto com alvo. 

```
$ sublist3r -d hackerone.com -o passive_subd.txt
```

Utilizando crt.sh

```
curl -s [http://crt.sh](https://t.co/3SNVM49m71)\?q\=\[http://facebook.com](https://t.co/zPr7Thk72Q)\&output\=json | jq -r '.[].name_value' | grep -Po '(\w+\.\w+\.\w+)$' | anew subdomains-faceboo.txt cat subdomains-faceboo.txt | wc -l
```


## Ativo

Possui contato direto com alvo.

```sh
$ xargs -a passive_subd.txt -I@ -P5 sh -c 'assetfinder @ | anew active_subd.txt'
$ subfinder -dL active_subd.txt -o subd.txt
```


### Httpx

```sh
$ httpx -l targets.txt -p portas_mais_comuns
```

-p: 80,443,3000,3333,4000,4442,4443,5000,8000,8080,8081