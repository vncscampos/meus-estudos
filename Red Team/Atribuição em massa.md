#attack #attack/api #api
## Como encontrar

- Encontrar parâmetros interessantes, como por exemplo parâmetros envolvidos nas propriedades da conta e adicionar ele em requisições
- Um ponto de partida é nas requisições de criação
- A extensão **Param Miner** do Burp pode ser para tentar encontrar parâmetros no JSON

## Testando crAPI

![[Pasted image 20240118111517.png]]
 
 Essa requisição mostra todos os produtos e suas propriedades. Como mostra no header da resposta, é possível usar método POST e talvez criar um produto.
 
![[Pasted image 20240118111947.png]]

Mudando o método e enviando sem alterar nada no body, o servidor retorna mostrando os atributos que são necessários. Então, reenviando a requisição mas agora com os atributos que pedia, é possível criar um novo produto.

Se alterar o valor do produto para um número negativo será criado um novo produto e ao comprar este produto o saldo na carteira irá mudar, pois provavelmente a lógica é `saldo - (preço do produto)`, porém como o preço é negativo, vai resultar em uma soma do saldo com o preço do produto.