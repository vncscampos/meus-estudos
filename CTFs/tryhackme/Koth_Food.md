# Koth Food CTF
https://tryhackme.com/room/kothfoodctf

## Flags

    thm{2f30841ff8d9646845295135adda8332} -> mysql tabela de usuarios
    thm{0c48608136e6f8c86aecdb5d4c3d7ba8} -> /var
    thm{7baf5aa8491a4b7b1c2d231a24aec575} -> /home/bread
    thm{58a3cb46855af54d0660b34fd20a04c1} -> /home/food
    thm{9f1ee18d3021d135b03b943cc58f34db} -> /root
    thm{5a926ab5d3561e976f4ae5a7e2d034fe} -> /home/tryhackme

## Recon
### NMAP

    PORT     STATE SERVICE VERSION
    22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    3306/tcp open  mysql   MySQL 5.7.29-0ubuntu0.18.04.1
    9999/tcp open  abyss?

A porta 9999 é uma pagina com a palavra king escrito

É possível conectar com o serviço mysql passando senha root

```sh
mysql -h <IP> -u root -p
Enter password: root
```

### Mysql

Da tabela de usuarios -> ramen:noodlesRTheBest -> conta ssh

## Privesc

Com o usuario ramen é possivel escrever no /etc/passwd com /usr/bin/vim.basic e atribuir uma nova senha para o root
