#ctf
## NMAP
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Rick is sup4r cool


## HTML
Username: R1ckRul3s:Wubbalubbadubdub

## GOBUSTER
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/assets (Status: 301)
/robots.txt (Status: 200)
/server-status (Status: 403)

## DIRB
/login.php
http://10.10.104.87/portal.php (CODE:302|SIZE:0)

## PANEL
rabbit hole
