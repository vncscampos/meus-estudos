#devsecops 
## Fundamentos do CI/CD

- Um **único repositório** deve ser usado para armazenar todos os arquivos
- As atualizações devem ser **menores e regularmente**
- **Build automatizado** à medida que atualizações chegam
- **Auto testes automatizados** antes das builds para garantir segurança e qualidade
- O código deve ser testado em **ambientes que imita** **a prod**
- Cada **dev** tem que ter **acesso às compilações e códigos** para entender e ver as alterações
- O **pipeline deve ser simplificado** para garantir que os deploys sejam feito sem nenhum risco de gerar instabilidade em prod

## Componentes

- **Developer workstation:** onde os devs desenvolvem e buildam o código
- **Source code storage:** gitlab, github, bitbucket...
- **Build orchestator:** coordena e gerencia a automação do build e do deploy (gitlab, jenkins)
- **Build agents:** build, testa e empacota o código (gitlab runners, jenkings agents).  Executa as jobs do CI/CD
- **Environments:** ambientes de desenvolvimento (testing, production, development)

## Protegendo código fonte

O primeiro passo para proteger a pipeline é proteger a fonte, contra:
- Adulteração não autorizada
- Divulgação não autorizada

E isso pode ser feito:
- Criando um controle de acesso baseado em grupo
- Definindo os níveis de acesso
- Evitar informações sensíveis no código
	- .gitignore
	- variáveis de ambiente
	- proteção de branch

## Protegendo a build

- Executar compilações em containers isolados
- Conceder permissões mínimas para ferramentas CI/CD
- Ferramentas de gerenciamento de secrets
- Armazenar artefatos de build em um registry seguro para evitar adulterações
- Integrar varredura de dependência
- Definir CI/CD pipeline as code
- Manter as ferramentas CI/CD e dependências sempre atualizadas
- Monitor os logs de atividades incomuns

## Protegendo o build server

- Firewalls
- VPN para acessar com segurança o servidor de locais remotos
- Autenticação baseada em token
- Chaves SSH
- Auditorias de segurança
- Remover credenciais/configurações padrões
- Configurar agentes de compilação para se comunicar apenas com o servidor de compilação
- Atualização regulares

## Protegendo a pipeline

- Limitar acesso a branch
- Revisar merge request
- Controle de acesso
- Monitorar e alertar

