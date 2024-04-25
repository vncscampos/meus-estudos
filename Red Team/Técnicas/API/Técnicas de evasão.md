#api #techniques 

## Strings terminators

Os símbolos de byte nulo se não forem filtrados, podem encerrar o controle de segurança da API. Estes símbolos para muitas linguagens de programação significa para o processamento.

```
%00
0x00
//
;
%
!
?
[]
%5B%5D
%09
%0a
%0b
%0c
%0e
```

Essas strings podem ser colocadas em diferentes partes de uma requisição. Ex:

```
POST /api/v1/user/profile/update
...

{"name":"vini", "pass": "%00' OR 1=1"}
```

## Case Switching

Alguns controles de segurança seguem a ortografia literal de um requisição e com *case switching* é possível burlar. Por exemplo, se em um endpoint que você esta testando um IDOR e precisa fazer 10000 requisições, provavelmente o WAF irá de bloquear porém se alterar entre letras maiúsculas e minúsculas no caminho:

```
POST /api/MyPrOfIlE
...

{uid=§0001§}
```

Outra situação é que o WAF ainda assim pode te bloquear para 100 requisições feitas para esse caminho `/api/Myprofile` por exemplo, então o que você pode fazer é saltar as letras: `/api/mYprofile`, `/api/myProfile`, `/api/mypRofile` etc.

## Codificar payloads

Tente codificar o payload uma vez só ou duas.

## Usando burp

No **Intruder** é possível processar o payload em **Payload Processing**.

![[Pasted image 20240120143739.png]]



