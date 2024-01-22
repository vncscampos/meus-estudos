#ctf
# ColddBox
https://tryhackme.com/room/colddboxeasy

## NMAP
- Portas abertas e serviços:
  - Porta 80/tcp aberta: HTTP
    - Servidor: Apache httpd 2.4.18 ((Ubuntu))
    - Detalhes HTTP:
      - Gerador: WordPress 4.1.31
      - Cabeçalho do servidor: Apache/2.4.18 (Ubuntu)
      - Título: ColddBox | One more machine
  - Porta 4512/tcp aberta: SSH
    - Versão: OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocolo 2.0)
  - Informações do Serviço: Sistema Operacional Linux; CPE: cpe:/o:linux:linux_kernel

## WPSCAN
- Tema WordPress em uso: twentyfifteen
  - Localização: [http://10.10.22.198/wp-content/themes/twentyfifteen/](http://10.10.22.198/wp-content/themes/twentyfifteen/)
  - [!] A versão está desatualizada, a última versão é 3.3
  - URL do estilo: [http://10.10.22.198/wp-content/themes/twentyfifteen/style.css?ver=4.1.31](http://10.10.22.198/wp-content/themes/twentyfifteen/style.css?ver=4.1.31)

- Usuário(s) identificado(s):
  - [+] c0ldd / 9876543210 -> Senha do WordPress
  - [+] hugo
  - [+] philip

## Linux Server
- Credenciais:
  - Usuário: c0ldd
  - Senha: cybersecurity
- Acesso root ao Vim/FTP
