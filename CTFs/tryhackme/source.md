# Source
https://tryhackme.com/room/source

## Recon

### NMAP

```
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## Exploit / Privesc

O Metasploit possui um módulo "linux/http/webmin_backdoor" que da acesso root à máquina
