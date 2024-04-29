#recon 

**Objetivo:** Encontrar urls, parâmetros que talvez ainda estejam em uso, api keys antigas e código antigo = bugs.

## Coletando todas as urls

Waybackurls:

```sh
$ cat hosts.txt | waybackurls | hakcheckurl | egrep -v '404' | awk '{print $2}' > urls.txt 
```

GetAllUrls:

```sh
$ echo "dominio.com" | gau
```


## Grep

O próximo passo após achar as urls é buscar por informações interessante como usuários, senhas, api, endpoints, queries etc.

```sh
$ cat urls.txt | grep api
```

A ferramente [gf](https://github.com/tomnomnom/gf) é um grep mais sofisticado que permite buscar por exemplo por urls que da para testar um idor, xss...

```sh
$ cat urls.txt | gf xss > xss_url.txt
```