#web #vuln

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

## Referência
https://portswigger.net/web-security/host-header