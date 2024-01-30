#api #techniques 

Quando a doc de uma API não é pública, o que nos resta é consumir essa API e descobrir os endpoints para criar uma doc.

Com o postman é possível criar fazer uma engenharia reversa graças ao proxy deles.

```
new collection > capture requests > proxy debug session
```

![[Pasted image 20240115140745.png]]

Mas a forma mais eficiente é usando o [MitmProxy](https://mitmproxy.org/) junto com o [MitmProxy2Swagger](https://pypi.org/project/mitmproxy2swagger/). Ela é mais eficiente por conseguir criar um swagger e ver os parâmetros aceitos de uma requisição, o body e seus atributos.

1. Executar o mitmweb
2. Ativar foxyproxy p porta 8080
3. Navegar pelo site
4. Salvar os flows que vai estar disponível 127.0.0.1:8081
5. `mitmproxy2swagger -i ~Downloads/flows -o spec.yml -p http://crapi.apisec.api -f flow`
6. Tem que editar o `spec.yml` e tirar o ignore: antes dos endpoints e rodar de novo comando
7. Importar no https://editor.swagger.io/

