#ctf
# Madness
https://tryhackme.com/room/madness

## Reconhecimento e exploração
Rodando NMAP:

    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
    80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))

No HTML existe uma imagem "thm.jpg" que não esta carregando, ao baixar ela e abrir o hexadecimal, nota-se que o cabeçalho está como PNG e não JPG.

Usando o ghex para mudar o hexadecimal __89 50 4E 47 0D 0A 1A 0A 00__ (PNG) para __FF D8 FF E0 00 10 4A 46 49 46 00 01__ (JPG) com base nesse [link](https://en.wikipedia.org/wiki/List_of_file_signatures?source=post_page-----8a8080672083--------------------------------)
é possível abrir a imagem e ver que o diretório escondido é */th1s_1s_h1dd3n*.

Neste diretório é preciso encontrar a secret que vai de 0-99. Usando o intruder do burp eu achei */th1s_1s_h1dd3n/?secret=73*.
Com isso aparece uma senha __y2RPJ4QaPF!B__ que é para ser usada para ver arquivos escondidos do thm.jpg

```sh
steghide --extract -sf thm.jpg -p y2RPJ4QaPF!B
```
hidden.txt -> usuario = wbxre -> ROT13 -> usuario = joker

A imagem do site também tem um arquivo escondido, o password.txt.

Com isso a conta ssh é joker:*axA&GF8dP

## Privesc
Ao rodar o [lse.sh](https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh) existe um arquivo suid /bin/screen-4.5.0.
Existe um [exploit](https://www.exploit-db.com/exploits/41154?source=post_page-----8a8080672083--------------------------------) para essa versao.

Rodando o exploit é GG.
