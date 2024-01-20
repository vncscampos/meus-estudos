#api 

Testar ativos impróprios é descobrir versões passadas de API que ainda podem ser executadas e encontrar fraquezas presentes. Encontrar versões que não estão incluídas na documentação da API será, no mínimo, uma vulnerabilidade por documentação técnica insuficiente (CWE-1059).

## Testando com postman

1. Entender o que diferencia um endpoint de uma versão para outra, pode ser por exemplo `/api/v1/user` -> `/api/v2/user`
2. Abrir collection, ir em Tests e clicar em "Status code: Code is 200"
3. Trocar a versão por uma varável de ambiente
4. Clicar em run