#devsecops 

**Static Application Security Testing** é usar ferramenta que automatiza análise de código durante seu desenvolvimento, ou seja, antes de ser compilado.

| Pros                                                          | Contras                                                               |
| ------------------------------------------------------------- | --------------------------------------------------------------------- |
| Fornece uma grande cobertura das funcionalidades da aplicação | Algumas partes não são avaliadas como por exemplo código de terceiros |
| Reporta exatamente onde está a vulnerabilidade                | Suscetível a falsos positivos                                         |
| Fácil de integrar com CI/CD                                   | Não identifica vulnerabilidades dinâmicas                             |
| Roda rapidamente                                              |                                                                       |

Cada ferramenta SAST roda diferente uma da outra, mas existe duas tarefas base para elas:
- **Converter o código em uma modelo abstrato**, geralmente Árvore Sintática Abstrata (AST), permitindo uma avaliação independente da linguagem
- **Análise do modelo abstrato** para achar falhas

O SAST poder ser implementado na etapa de desenvolvimento do SSDLC de duas formas:
- **Integração com CI/CD:** o scan vai executar depois de um merge ou um PR aberto
- **Integração com IDE:** é uma forma mais simples que evita gargalos no futuro

Integrar com CI/CD e com a IDE também é válido já que na IDE é feita análise básica da estrutura do código e no CI/CD a análise é capaz de seguir o fluxo do código. 