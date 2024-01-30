#recon 


**Objetivo**: coletar todas informações possíveis -> domínio, *acquisitions*, informações sobre registros, ASN, DNS etc.

## OSINT

- Coletar informações sobre os colaboradores → Linkedin, Github, Instagram, Facebook;
- Vagas de emprego;
- Informações sobre os projetos → [trello](https://trello.com/);
- Coletar endereços de email → [hunter](https://hunter.io/);
- Vazamento de dados (Leaks) → [have i been pwned](https://haveibeenpwned.com/), [dehashed](https://www.dehashed.com/), [pastebin](https://pastebin.com/);
- Vazamento de dados (Leaks) na Deep Web → [pwndb](http://pwndb2am4tzkvold.onion/);
- Cache de sites → [wayback machine](http://web.archive.org/);
- Search engine, data archive, vulnerability scanner → [intelx](https://intelx.io/)
- theHarvester
- Shodan
- Censys

### Dorks

https://dorks.faisalahmed.me/#

## Whois e DNS

```sh
$ whois dominio.com
$ dnsenum site.com
$ echo site.com | dnsx -silent -a -resp
```

- [DNSdumpser](https://dnsdumpster.com/)

## Aquisições

As *acquisitions*, são aquisições corporativas, ou seja, compras de uma empresa por outra. No contexto de **bug bounty** ou **pentest**, descobrir as informações sobre as aquisições pode ser relevante por várias razões:
- Expandir a superfície de ataque
- Herança de vulnerabilidades
- Descoberta de subdomínios
- Possibilidade de informações sensíveis compartilhada

## ASN

O ASN (Autonomous System Number) delimita um prefixo de endereços IPs que está sob controle de uma entidade (sistema autônomo). 

- [BGP](https://bgp.he.net/)

Descobrindo o número do sistema é possível identificar os intervalos de IP dentro desse ASN.

```sh
$ whois -h whois.radb.net  -- '-i origin AS714' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq
```

Com essa lista de IP é feito um dns reverso (ip -> dominio).

```sh
$ echo 17.0.0.0/8 | mapcidr -silent | dnsx -ptr -resp-only -o output.txt
```

