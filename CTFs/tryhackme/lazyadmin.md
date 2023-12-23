# Lazy Admin
https://tryhackme.com/room/lazyadmin

## Recon

### NMAP

    PORT   STATE SERVICE
    22/tcp open  ssh
    80/tcp open  http

### Gobuster

- Existe um pasta /content onde está hospedado o CMS SweetRice.
- Rodando gobuster nesse /content, é descoberto alguns caminhos interessantes como /as onde há um login e /inc onde existe um arquivo de backup em sql.
- No arquivo backup existe as credenciais manager:Password123

### Reverse Shell

- Na dashboard existe uma página chamada "ads" que é possível salvar um arquivo .php
- O arquivo salvo fica em /content/inc/ads


## Privesc

### www-data -> root

- /home/itguy/mysql_login.txt -> rice:randompass
- /home/itguy/backup.pl é possível rodar como root sem precisar de senha
- O arquivo executa o /etc/copy.sh que possui permissão "rwx" para qualquer usuário

```sh
echo "/bin/bash -i" > /etc/copy.sh
sudo -u root /usr/bin/perl /home/itguy/backup.pl
```

GG
