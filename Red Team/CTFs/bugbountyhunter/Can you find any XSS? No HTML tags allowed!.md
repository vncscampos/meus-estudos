#ctf 

https://www.bugbountyhunter.com/challenge?id=1

A página não aceita tag completa e eventos como onload, onfocus, onclick também são excluídos completamente.

O XSS que consegui foi incorporar uma tag `<a>` que encaminha para o meu site.

```html
<<a id=x tabindex=x href="http://site-do-mal.com">site-do-bem.com
```

