#recon 

**Objetivo:** encontrar diretórios escondidos, arquivos, queries etc

## Procurar diretórios

Gobuster:

```sh
$ gobuster dir -u https://site.com -w /usr/share/wordlists/dirb/common.txt
```

FFuf -> [[Fuzzing]]