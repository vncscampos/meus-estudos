#ctf 

https://portswigger.net/web-security/ssrf/lab-ssrf-with-whitelist-filter

O endpoint suscetível a SSRF é `/product/stock`. Ele possui um atributo chamado `stockApi` que passa uma URL codificada:

```
http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D2%26storeId%3D2
```

Quando enviamos uma outra URL por exemplo `http://localhost` o servidor responde: *"External stock check host must be ==stock.weliketoshop.net=="*. O que pode significar que a aplicação verifica se a URL enviada contém dentro dela `stock.weliketopshop.net`.

Enviando `http://localhost@stock.weliketoshop.net` aparece um outro erro "*Could not connect to external stock check service*". 

Acrescentando # antes de @, a primeira mensagem de erro volta a aparecer, porém se codificar duas vezes `http://localhost%2523@stock.weliketoshop.net` a requisição retorna 200 OK.

Com isso dá para entender um pouco como funciona a aplicação. Ela considera tudo antes da # como a url, da @ em diante, como uma referência a um elemento na página.


```
http://localhost%2523@stock.weliketoshop.net/admin/delete?username=admin
```