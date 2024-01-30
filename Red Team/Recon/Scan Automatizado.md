#recon #techniques 

**Objetivo:** Achar vulnerabilidades

Scanners automáticos não possuem resultados muitos precisos, além de gerar muitos logs para o alvo, porém ajuda a ter um ponto de partida para explorar algo.

## Nikto

```sh
$ nikto -h https://site.com -o report.html
```

## Nuclei

```sh
$ nuclei -target site.com
```

```sh
$ nuclei -list hosts.txt -o output.json
```

## Dalfox (xss scan)


```sh
$ dalfox url https://site.com/?cat=123 -b https://your-callback-url
```

```sh
$ cat urls.txt | dalfox pipe
```