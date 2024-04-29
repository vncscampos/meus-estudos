#ctf 

https://app.hackthebox.com/challenges/ProxyAsAService

Para conseguir pegar a flag, precisamos acessar a rota `/debug/environment`. Porém ela só pode ser acessada localmente.

```python
def is_from_localhost(func):
	@functools.wraps(func)
	def check_ip(*args, **kwargs):
		if request.remote_addr != '127.0.0.1':
			return abort(403)
		return func(*args, **kwargs)
	return check_ip
```

A rota principal concatena o que está na query `url` com o `SITE_NAME`. 

```python
SITE_NAME = 'reddit.com'

@proxy_api.route('/', methods=['GET', 'POST'])
def proxy():
	url = request.args.get('url')

	if not url:
		cat_meme_subreddits = [
			'/r/cats/',
			'/r/catpictures',
			'/r/catvideos/'
		]
	
		random_subreddit = random.choice(cat_meme_subreddits)
		return redirect(url_for('.proxy', url=random_subreddit))

	target_url = f'http://{SITE_NAME}{url}'
	response, headers = proxy_req(target_url)
	return Response(response.content, response.status_code, headers.items())
```

É possível burlar essa concatenação e fazer o app acessar o nosso site manipulando a url com `@`, assim a requisição vai para o que veio depois do sinal ([[SSRF]]): 

```
http://reddit.com@127.0.0.1:1337/debug/environment
```

Ainda há mais um passo para fazer, pois ao usar essa url é retornado 403. A rota principal verifica se na url contém as seguintes strings: `RESTRICTED_URLS = ['localhost', '127.', '192.168.', '10.', '172.']`. Então é necessário burlar e uma forma simples é trocar `127.0.0.1` para 0

```
/?url=@0:1337/debug/environment
```

GG