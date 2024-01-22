#ctf
# Opacity
https://tryhackme.com/room/opacity

## Recon e exploração

### NMAP

    PORT    STATE SERVICE     VERSION
    22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
    80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))
    139/tcp open  netbios-ssn Samba smbd 4.6.2
    445/tcp open  netbios-ssn Samba smbd 4.6.2
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
### Gobuster

O gobuster achou um diretorio chamado /cloud onde é possível fazer upload de imagens (jpg).

### Local File Unrestricted

Salvando uma shell reversa com extensão ".php#.jpg" é possível ganhar acesso à máquina

## Privesc

O arquivo /var/www/html/login.php possui algumas credenciais para logar no app

    $logins = array('admin' => 'oncloud9','root' => 'oncloud9','administrator' => 'oncloud9');
    
### Enumeração

- Keepass database files -> /opt/dataset.kdbx

### www-data -> sysadmin

Baixando o banco keepass encontrado e quebrando com o john, é extraido a senha master -> 741852963

```sh
keepass2john data.kdbx > keepasshash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt keepasshash.txt
```

Usando o keepass2 para abrir o banco é possível extrair a senha do usuario sysadmin e logar pelo ssh.


### sysadmin -> root

Lendo o arquivo "script.php" que o root executa, ele faz uso do arquivo "backup.inc.php" que esta na pasta lib no mesmo diretorio. O usuário sysadmin não consegue alterar diretamente esse arquivo mas consegue criar arquivos na pasta lib. Com isso, criando um "backup.inc.php" fake que realiza um reverse shell e enviando para pasta lib, basta voltar e rodar o script.php para conseguir acesso ao root.



