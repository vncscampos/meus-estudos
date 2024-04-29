#attack #attack/web #web

O header Host define o domínio que o cliente quer acessar, sendo necessário quando o servidor web possui vários domínios ou aplicativos.

- **Virtual hosting:** hosts diferentes porém mesmo servidor
- **Tráfego de roteamento intermediário:** sites hospedados em servidores diferentes porém todo o tráfego passa por um sistema intermediário (proxy, balanceador de carga). Neste caso, todos os domínios apontam para um mesmo endereço

O ataque ocorre devido à injeção de payloads no valor do cabeçalho Host, decorrente da falha na devida validação pelos servidores.

## Como identificar

Se conseguir mudar o header host e a requisição ainda assim retornar 200 então pode ser vulnerável.

- **Teste 1:** trocar o host
	- pode receber um `Invalid Host header` mas não necessariamente significa que não é vulnerável

* **Teste 2:** Se for possível fornecer uma porta não numérica, pode tentar injetar payloads através da porta

- **Teste 3:** fornecer um subdomínio que ja foi comprometido

- **Teste 4:** duplicar host headers

```
GET https://vulnerable-website.com/ HTTP/1.1 
Host: bad-stuff-here

ou

GET /example HTTP/1.1 
Host: vulnerable-website.com 
Host: bad-stuff-here

ou

GET /example HTTP/1.1 
	Host: bad-stuff-here 
Host: vulnerable-website.com
```

- **Teste 5:** algumas aplicações podem injetar os headers `X-Forwarded-Host`, `X-Host`, `X-Forwarded-Server`, `X-HTTP-Host-Override`,  que também são alvo de injeção de payloads maliciosos.

```
GET /example HTTP/1.1 
Host: vulnerable-website.com 
X-Forwarded-Host: bad-stuff-here
```

## Vulnerabilidades

### Password reset poisoning

Essa técnica consiste em manipular a redefinição de senha que gera um link para um domínio que você tem controle. Isso pode ser usado para roubar os tokens secretos necessários para redefinir senha.

1. Conseguir o email ou username da vítima
2. Fazer uma requisição de redefinição de senha com o dado da vítima e com host header alterado para um domínio de controle
3. O servidor vai enviar um email para vítima com o token, o link deve ser aberto de alguma forma, seja pela própria pessoa ou até mesmo pelo scanner de antivírus
4. O hacker vai conseguir pegar o token e mudar a senha

Lab: [[Host header injection - password reset via dangling markup]]

### Web cache poisoning

[[Web cache poisoning - with an unkeyed header]]

### Server-side

Algumas ataques clássicos de server-sides podem ser explorados como por exemplo SQLi.

### Acessando funcionalidade restrita

Algumas funcionalidades podem estar restritas a usuários internos. Por exemplo, neste [lab](https://portswigger.net/web-security/host-header/exploiting/lab-host-header-authentication-bypass) a página /admin está disponível apenas para usuários internos, então alterando o host para `localhost` é possível acessar a página.

E até mesmo sites internos podem ser hospedados no mesmo servidor onde está hospedados site de acesso público, basta descobrir o domínio.

### Routing-based [[SSRF]]

Essa vulnerabilidade explora componentes intermediários presentes em arquiteturas baseada em nuvem como proxies reversos ou balanceadores de carga. Estes componentes são implantados geralmente para encaminhar requisições para o servidor, porém se esse encaminhamento for com base no cabeçalho **host** que não é validado então "Houston, we have a problem.".

Os componentes são alvos interessantes, porque ocupam uma posição privilegiada da rede interna tendo acesso a grande parte dela (se não toda).

O **Burp Collaborator** (versão Pro :c) ajuda a identificar essas vulnerabilidades. Se fornecer o domínio do seu servidor Collaborator no cabeçalho Host e posteriormente receber uma consulta DNS do servidor-alvo ou de outro sistema no caminho, isso indica que você pode ser capaz de rotear requisições para domínios arbitrários.

## Referência
https://portswigger.net/web-security/host-header