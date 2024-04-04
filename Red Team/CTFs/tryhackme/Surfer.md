#ctf 

https://tryhackme.com/r/room/surfer

## Recon

```
PORT
22/tcp
80/tcp
443/tcp
```

- robots.txt -> /backup/chat.txt -> admin:admin

## Exploit

A ferramenta **export2pdf** está sendo utilizada para transformar um arquivo local em pdf através da requisição `/export2pdf.php`. O arquivo que é para ser convertido é passado  no body `url=http://127.0.0.1/server-info.php`.

Olhando a dashboard existe um arquivo em `/internal/admin.php` que é possível acessar acessando a rota `/export2pdf.php`  -> `url=http://127.0.0.1/internal/admin.php`. 

Nele é onde está a flag. GG!
