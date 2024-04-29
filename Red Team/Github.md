#recon 

**Objetivo:** Achar os repositórios de uma organização e suas credenciais (password, api_key, token etc)
## Trufflehog


```sh
$ docker run --rm -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=<organizacao>
```

## Githound

Busca automatizada no github.

```sh
$ echo "\"uberinternal.com\"" | githound --dig-files --dig-commits --many-results --languages common-languages.txt --threads 100
```

```sh
$ git-hound --subdomain-file subdominios.txt
```

