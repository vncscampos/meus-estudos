#soc #siem 

O **Elastic Stack** é uma coleção de diferentes componentes que conversam entre sí para colher dados de várias origens e formatos para poder realizar uma busca, análise e visualização em tempo real.

- **Beats:** agente que coleta os dados
- **Logstash:** processa os dados e envia para o Elasticsearch
- **Elasticsearch:** mecanismo de pesquisa
- **Kibana:** visualização dos dados no web para poder investigar, analisar...


A kibana possui 3 áreas importantes

## Discover tab

![[Pasted image 20240430144508.png]]

Todo log gerado pode ser visualizado e analisado/filtrado aqui. Por padrão ele precisa saber o **Index Pattern** (3). Esse padrão informa quais dados do Elasticsearch queremos explorar. Cada padrão tem uma estrutura diferente. 

O filtro pode ser adicionado setando os fields ou também através da barra de pesquisa com texto livre tipo "Vinicius" ou baseado no filed -> UserName: "Vinicius". Também é possível usar operadores (AND, OR, NOT)

## Visualize library

Nessa tab da para criar gráficos, tabelas deixando mais apresentável os dados

## Dashboard

A dashboard é construída do seu jeito e as visualizações criadas podem ser adicionadas na dashboard.