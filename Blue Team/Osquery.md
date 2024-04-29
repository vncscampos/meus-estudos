#soc #network #endpoint #endpoint/sec

O Osquery é uma ferramenta open-source que converte as informações que um sistema operacional gera em um banco de dados relacional (semelhante SQLite).

Para iniciar no modo interativo: `osqueryi`.

Alguns comandos úteis (precisa do . antes quando não é uma query):

- `.help`
- `.tables process`
- `.schema yar`

Com ele é possível ver por exemplo:

- Detalhes de processos:

```sql
select * from processes;
```

Os usuários do sistema:

```sql
select uid, username, shell, directory from users;
```

- Executáveis que um usuário rodou no Windows

```sql
select path,sid from userassist;
```

Entre várias outras consultas de tabela. E essas tabelas podem ser encontradas [aqui](https://osquery.io/schema)