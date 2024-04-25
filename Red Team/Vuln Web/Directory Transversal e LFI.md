#web #vuln

**Directory Transversal** ou **path transversal** permite o hacker ler arquivos do servidor que está rodando a aplicação, o que inclui:

- Códigos e dados
- Credenciais
- Arquivos do SO

**LFI:** ler arquivos locais
**RFI:** ler arquivos remotos

Por exemplo:

```
https://website.com/loadImage?filename=../../../etc/passwd
```

### 25 parâmetros mais comuns

```
?cat={payload}
?dir={payload}
?action={payload}
?board={payload}
?date={payload}
?detail={payload}
?file={payload}
?download={payload}
?path={payload}
?folder={payload}
?prefix={payload}
?include={payload}
?page={payload}
?inc={payload}
?locate={payload}
?show={payload}
?doc={payload}
?site={payload}
?type={payload}
?view={payload}
?content={payload}
?document={payload}
?layout={payload}
?mod={payload}
?conf={payload}
```


## Bypass

- php filter
- null bytye
- url encode
- duplicar ../ -> ....//


## Referência

https://book.hacktricks.xyz/pentesting-web/file-inclusion
https://www.synacktiv.com/en/publications/php-filter-chains-file-read-from-error-based-oracle.html