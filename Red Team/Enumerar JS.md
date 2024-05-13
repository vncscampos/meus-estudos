#recon 

**Objetivo:** Achar endpoints, URLs, APIs, hard-coded credentials, vulnerabilidades etc

### Passo 1 - Extrair JS

```sh
$ waybackurls hosts.txt | grep "\\.js" | xargs -n1 -I@ curl -k @ | tee -a files.txt
$ cat domains.txt | getJS
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

**Keywords comum:** `api`, `apiKey`, `admin`, `token`, `url:`, `getParameter()`, `parameter`, `onreadystatechange`, `POST`, `GET`, `setRequestHeader()`, `dominio.com`, `.headers`

**Keywords para achar bugs, xss, redirects:** `.innerHTML`, `document.write()`, `document.cookie`, `location.href`, `redirectUrl`, `messageListener`, `postMessage`, `windows.hash`, `eval(`
## Extra

- Retire.js: é uma extensão do Firefox que mostra as CVEs das bibliotecas JS do site