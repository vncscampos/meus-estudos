#recon #attack 

**Objetivo:** Tomar posse de um subdomínio

Este ataque ocorre quando o invasor consegue controlar um subdomínio. Alguma das causas são:

- Um subdomínio pode ser associado a um serviço cloud (AWS S3, Heroku, Github Pages) e o proprietário desativou ou removeu a configuração do serviço;
- Um domínio estava anteriormente associado a uma conta ou serviço que expirou, foi cancelado ou não foi renovado, o subdomínio pode ficar disponível para registro por outras pessoas;
- Se um domínio foi transferido de um provedor de serviços para outro e as configurações de DNS não foram atualizadas corretamente;

Os domínios possuem um registro DNS chamado **CNAME**, que serve para associar um subdomínio a outro domínio (ou subdomínio). A vulnerabilidade acontece quando um subdomínio possui um nome canônico (CNAME) mas não possui um host ativo, com isso o invasor pode assumir o controle desse subdomínio fornecendo seu próprio host virtual.

O controle de um subdomínio pode levar a roubo de dados e até mesmo perda financeira, e com isso, prejudicar a reputação de uma empresa.

## Como encontrar

Este ataque costuma estar associado (não necessariamente) a **serviços cloud**.

Depois de ter feito a enumeração de subdomínios o próximo passo é enumerar os registros CNAME e analisar o que achar interessante. Por exemplo, se com o primeiro comando abaixo vi que existe um um subdomínio que aponta para um bucket s3 da amazon (algo parecido com *{bucketname}.s3.amazonaws.com*), o segundo comando irá confirmar se existe um host ativo para esse subdomínio. Se retornar uma mensagem de erro como por exemplo "NoSuchBucket", então é possível pegar esse subdomínio.

```sh
$ httpx -l subdominios.txt -cname cnames.txt
$ httpx -l subdominios.txt -status-code -title -o hosts.txt 
```

Existe também scripts que automatizam essa busca por subdomínios vulneráveis como:
- [SubOver](https://github.com/Ice3man543/SubOver)
- [Subzy](https://github.com/PentestPad/subzy)

```sh
$ ./subover -l subdominios.txt -v
$ subzy run --targets subdominios.txt
```

OBS: scripts automatizados costumam gerar falsos positivos.

## POC

Após achar um subdomínio vulnerável é preciso criar a POC e para isso o repositório [Can I takeover XYZ?](https://github.com/EdOverflow/can-i-take-over-xyz) fornece instruções para vários tipos de serviços cloud.