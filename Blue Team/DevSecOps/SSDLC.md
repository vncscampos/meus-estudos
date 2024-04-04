#devsecops 

*Secure Software Development lifecycle* envolve adotar medidas de segurança em todas as fases do ciclo de desenvolvimento.

## Avaliação de risco

Um risco é uma possível ameaça ser explorada, causando um impacto negativo ao alvo. A avaliação de risco (*Risk assessment*) é usada para identificar e determinar o nível desse risco.

1. Primeiro passo é assumir que o software será alvo de ataques e considerar os fatores que motivam o agente de ameaça. Listar os valores dos dados, o nível de segurança da empresa, os clientes, o alcance, isso tudo para definir o nível aceitável de risco.
2. Esse passo é a avaliação, partindo do pior cenário, que é o cenário que o atacante teve sucesso. O que poderia acontecer? Qual relevância das coisas que ele pode fazer? Se ele conseguir acessar um sistema de monitoramento é risco de alto nível que não pode ser aceitável e é melhor mitigar. 
3. Considerar o número de pessoas afetadas, a facilidade propagação e a disponibilidade do alvo

### Tipos de avaliação de risco

- **Qualitativo:** avaliar os riscos em níveis como "Baixo", "Médio" e "Alto" -> `Risco: Impacto x Probabilidade`
- **Quantitativo:** avaliar os riscos através de faixas de números

![[Pasted image 20240306113844.png]]
Imagem do tryhackme
## Threat Modelling

- A modelagem de ameaças é melhor integrada à fase de design antes de qualquer código escrito, pois quando os problemas são identificados precocemente e resolvidos, economizam custos de correção posteriormente.
- O processo envolve identificar e utilizar técnicas para mitigar para que os dados ou ativos coletados na fase anterior sejam protegidos.

### STRIDE

- Modelo de ameaças utilizado pela Microsoft
- Profissionais de segurança que realizam o STRIDE procuram responder "O que poderia dar errado com este sistema?"
- **S**poofing **T**ampering **R**epudiation **I**nformation Disclosure **D**oS **E**scalation of Privilege

### DREAD

- Também criada pela Microsoft que pode ser um complemento ao modelo STRIDE
- Classifica as ameaças atribuindo a gravidade e prioridade

1. **Damage:** refere o dano que uma ameaça pode causar em uma escala de 0 a 10, onde: 
	- 0 ->  nenhum dano
	- 5 -> information disclosure
	- 8 -> dados de usuário comprometido
	- 9 -> dados internos comprometidos
	- 10 -> indisponibilidade do sistema
2. **Reproducibility:** o quão facilmente o hacker pode replicar uma ameaça
	- 0 -> quase impossível
	- 5 -> complexo
	- 7,5 -> para usuário autenticado
	- 10 -> muito rápido e sem autenticação
3. **Exploitability:** o quão fácil seria para lançar o ataque
	- 2.5 -> precisa de habilidades de programação avançada
	- 5 -> pode ser explorada com programas disponíveis
	- 9 -> precisa só de uma ferramenta de web proxy
	- 10 -> pode ser explorada pelo próprio browser
4. **Affected Users:** descreve o número de usuários afetados
	- 0 -> não vai ter usuários afetados
	- 2.5 -> um usuário
	- 6 -> um grupo pequeno
	- 9 -> usuários importantes como admins
	- 10 -> todos os usuários
5. **Discoverability:** a dificuldade de descobrir essa falha no sistema
	- 0 -> vai ser desafiador
	- 5 -> pode ser descoberta por analise de pacote HTTP
	- 8 -> é facil de achar
	- 10 -> é visível na barra de endereço do browser

### PASTA

- **Process for Attack Simulation and Threat Analysis**
- Alinha requisitos técnicos com objetivos de negócios
- Envolve o processo de modelagem de ameaças, desde análise das ameaças até a identificação de formas de mitigá-las, mas em nível estratégico e sob a visão do hacker

1. **Definir Objetivos**
2. **Definir o escopo técnico**: montar diagramas da arquitetura física e lógica
3. **Decomposição e análise:** são identificados os vetores de ameaça e avaliando quais componentes são mais suscetíveis a ataque
4. **Threat analysis:** informações extraídas da inteligência de ameaças
5. **Análise de vulnerabilidades e fraquezas:** identifica falhas no aplicativo e enumera vulnerabilidade. É altamente recomendável adicionar mitigação à ameaça identificada nesta etapa
6. **Enumeração e Modelagem de Ataque/Exploração:** esta etapa simula todas as informações enumeradas extraídas de todos os passos anteriores, mapeando toda a superfície de ataque da aplicação e em seguida todos os vetores de ataque para diferentes nós.
7. **Análise de risco e impacto:** com base nas etapas anteriores, todo o escopo afetado é analisado para ser documentado e em seguida mitigar o risco

## Secure Coding

### Code Analysis

- **Análise estática** examina o código antes de executar o programa
- **Análise dinâmica** olha o código fonte enquanto o programa está em execução

#### SAST

- **Static Application Security Testing**
- Detecta sinais de vulnerabilidades antes do código ser compilado -> **white box**

#### SCA

- **Software Composition Analysis**
- Faz scan das dependências

#### DAST

- **Dynamic Application Security Testing**
- Detecta vulnerabilidades em tempo real (foi pro deploy), sem informações internas do app ou código fonte -> **black box**
- Funciona simulando ataques automatizados em um app, imitando um invasor

#### IAST

- **Interactive Application Security Testing**
- Detecta vulnerabilidades em tempo real, assim como DAST
- Pode identificar a linha de código que causa problemas -> **gray box**

#### RASP

- **Runtime Application Self Protection**
- Usado após o lançamento para identificar padrões de comportamento dos usuários finais, do tráfego interno/externo

![[Pasted image 20240307114037.png]]imagem do tryhackme

## Security Assessment

- Pode ser implementada em todas as fases do SSDLC porém é mais comum nas fases finais de **Operations** e **Maintenance**
- **Avaliação de vulnerabilidade** normalmente usam scans (OpenVAS, Nessus etc) para encontrar vulnerabilidades mas não validam nem simulam os resultados para provar que são exploráveis
- **Pentest** vai mais fundo nos testes de vulnerabilidades para conseguir invadir os sistemas