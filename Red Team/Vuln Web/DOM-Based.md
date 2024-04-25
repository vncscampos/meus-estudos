#web #vuln

DOM (**Document Object Model**) é uma representação hierárquica (uma árvore) dos elementos de uma página. O JS pode manipular os nós e os objetos da DOM e também suas propriedades. Isso não é uma falha, até porque é assim que funciona os websites, porém quando o JS manipula dados inseguros, isso pode ativar vários tipos de ataque.

### Source

*Source* são propriedades controladas pelo atacante, como por exemplo a propriedade `location.search` que lê o input da **query string**.

```
document.URL
document.documentURI
document.URLUnencoded
document.baseURI
location
document.cookie
document.referrer
window.name
history.pushState
history.replaceState
localStorage
sessionStorage
IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB)
Database
```

### Sink

*Sink* são funções JS ou objeto DOM potencialmente perigosas, que podem causar efeitos indesejáveis se os dados controlados pelo atacante for passado para ele. Por exemplo: `eval()`, `document.innerHTML` etc.

## DOM Invader

É uma ferramenta no navegador do Burp que detecta automaticamente *sources* e *sinks*. Ela está instalada no DevTools.