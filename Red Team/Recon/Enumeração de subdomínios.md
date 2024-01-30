#recon 

## Passivo

Sem contato direto com alvo.

```
$ sublist3r -d hackerone.com -o passive_subd.txt
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