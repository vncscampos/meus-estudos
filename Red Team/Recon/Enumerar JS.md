#recon 

**Objetivo:** Achar endpoints, urls, api, eval(), hard-coded credentials, vulnerabilidades etc

### Passo 1 - Extrair JS

```sh
$ waybackurls hosts.txt | grep "\\.js" | xargs -n1 -I@ curl -k @ | tee -a files.txt
```

### Passo 2 - jsbeautifier

O `jsbeautifier` deixa o código mais legível além de desofuscar o código.

```sh
$ pip install jsbeautifier
$ js-beautify -o js_beauty.txt files.txt
```

### Passo 3 - grep

```sh
$ cat js_beauty.txt | grep api
$ cat js_beauty.txt | grep token
$ cat js_beauty.txt | grep admin
```
## Extra

- Retire.js: é uma extensão do Firefox que mostra as CVEs das bibliotecas JS do site