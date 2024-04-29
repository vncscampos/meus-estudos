#attack #attack/api

A melhor maneira de descobrir pontos de injeção em uma API é através de [[Fuzzing]], ou seja, enviar vários tipos de entrada inesperada (número, símbolo, comandos, query, até emoji) para um endpoint para provocar uma resposta não intencional.

Alguns alvos de fuzzing:
- Headers
- Parâmetro de consulta
- POST/PUT

Quanto mais você conhecer a aplicação, mais direcionado vai ser o fuzzing e menos "barulho" você vai fazer, por exemplo, se eu sei que o banco usado é MySQL, não vou querer que o **sqlmap** use queries SQLite.

## SQLi

Se um endpoint que faz uso de consultas SQL a uma API não filtrar ou sanitizar o input, quaisquer consultas SQL passadas vão para o banco de dados permitindo que o invasor interaja remotamente com o banco, isto é [[SQL Injection]].

## NoSQLi

[[NoSQLi]]

## Command Injection

[[Command Injection]] 

## Postman

O postman é muito útil para fazer fuzzing com vários endpoints graças ao **Collection Runner**. Ferramentas como **WFuzz** são melhores em procurar em requisições individuais.

1. Selecionar os endpoints que possuem inputs e que provavelmente interaja com o banco
2. Criar uma variável de ambiente `fuzz` com valores aleatórios
3. Setar essa variável nos inputs e parâmetros
4. Em Collection Runner clicar em "save responses"
5. Rodar