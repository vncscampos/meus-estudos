https://tryhackme.com/room/vulnnetinternal

## Recon

Usando NMAP:

```
PORT      STATE    SERVICE     VERSION  
22/tcp    open     ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
139/tcp   open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  
445/tcp   open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  
873/tcp   open     rsync       (protocol version 31)  
2049/tcp  open     nfs
6379/tcp  open     redis       Redis key-value store  
Service Info: Host: VULNNET-INTERNAL; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

No SMB existem 3 arquivos descobertos através do smbclient:
- business-req.txt  
- data.txt  
- services.txt -> possui a primeira flag

Através do módulo `use auxiliary/scanner/rsync/modules_list` do metasploit para enumerar pastas compartilhadas do rsync, descobri um arquivo chamado "files" mas é preciso senha para poder acessar (bruteforce não dá).

Como a porta 2049 está aberta, tentei buscar alguma coisa dela, e usando o comando `showmount -e IP`, existe um diretório que pode ser acessado remotamente `/opt/conf`. Ao montar usando os comandos abaixo, obtive o acesso a arquivos de configurações como por exemplo a senha do redis **B65Hx562F@ggAZ@F**.

```sh
mkdir /mnt/vulnnet
mount -o rw IP:/opt/conf /mnt/vulnnet
```

### Explorando redis

Usando o módulo do metasploit `uxiliary/gather/redis_extractor` é possível obter a segunda flag.

Nele há um base64 que ao decodificar nos dá o que é necessário para autenticar no rsync `Authorization for rsync://rsync-connect@127.0.0.1 with password Hcg3HP67@TW@Bc72v`

Depois de ter explorado as pastas do rsync, voltei a explorar redis para ganhar acesso ao alvo. Nessa versão do redis 4.0.9 consegui realizar um shell reverso com ajuda desse repositório [redis-rogue-server](https://github.com/n0b0dyCN/redis-rogue-server/tree/master). 
### Explorando rsync

Rodando o comando `rsync -av rsync://rsync-connect@IP/files .` é baixado tudo que está em `/home/sys-internal` na máquina alvo e é onde está a terceira flag.

Não achei nada que pudesse ajudar a ganhar acesso do alvo. Nem dumpzilla descobre algo da pasta `.mozilla`

## Privesc
