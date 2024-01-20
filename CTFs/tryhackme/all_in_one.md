#ctf
# All in ONe
https://tryhackme.com/room/allinonemj

## Passo 1: Utilizando NMAP para Identificar Portas e Serviços
```shell
nmap -sV -p 21,22,80 <IP_DO_SERVIDOR>
```

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

## Passo 2: Escaneando o WordPress com WPScan
```shell
wpscan --url http://<IP_DO_SERVIDOR> — enumerate u, ap
```
u: enumar os usuários
ap: detecta todos os plugins (all-plugins)

WordPress version 5.5.1 identified
WordPress theme in use: twentytwenty 1.5
User(s) Identified -> elyana / H@ckme@123


## Linux
elyana - E@syR18ght
