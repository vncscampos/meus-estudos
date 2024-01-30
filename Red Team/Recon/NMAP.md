#recon 

## Identificar hosts ativos

```shell
$ nmap -sn <ip> -oG hosts
```

```shell
$ xargs -a list_cidr.txt -I@ -P5 sh -c 'nmap -sn @ | anew hosts_ativos.txt'
```

## Escaneando os hosts encontrados

```sh
$ nmap -v -Pn <ip>
$ nmap -Pn -iL hosts_ativos.txt
```

O `-Pn` pula a verificação de descoberta de host.

### TCP Scan

```sh
$ nmap -v -sT -p- -Pn <ip>
```

### UDP Scan

```sh
$ nmap -sUV -Pn <ip>
```

### Scan de vulnerabilidade

```sh
$ nmap -sV --script vuln <ip>
```

### Fingerprinting

```sh
$ nmap -v -sV -O -Pn <ip>
```

### Wildcards

Usar wildcards pode simplificar o escaneamento, por exemplo, você quer escanear somente portas relacionadas a http.

```sh
$ nmap -p http* <ip>
```

### Todas as portas

```sh
$ nmap -p- -Pn --open <ip> --min-rate=800
```
## Bypass

As vezes o firewall pode dificultar a nossa vida, e uma forma de ver as portas abertas é com a flag `SYN`.

```sh
$ nmap -sS -Pn <ip>
```

![[nmap_response.png]]

Firewall má configurado é possível contornar usando uma porta de origem no scan nmap.

```sh
nmap -sS -g 53 -Pn <ip>
```

A opção `-f` fragmenta o pacote IP.

```sh
$ nmap -f -Pn <ip>
```

É possível gerar IPs aleatórios falsos para ofuscar a origem da varredura.

```sh
$ nmap -sS -D RND:20 --top-ports=10 --open -Pn <ip>
```

