
## Threats

Uma ameaça é um evento indesejado que pode acontecer ou não, colocando em risco os ativos de uma entidade e, potencialmente, prejudicando-a. Essas ameaças podem ser de origem natural (incêndios, desabamentos), não intencional (*delete* sem *where* :s) e intencional, que pode vir de alguém de fora e até mesmo de dentro da organização.

## Malware

Malware = **mal**icious + soft**ware**

Estes programas maliciosos podem ser usados para diversos fins, como roubar informações, deixar um sistema inoperável, prejudicar a performance etc. E também possui diferentes caminhos para acessar um sistema como por exemplo: SMS, pen-drives, anexo de email, bluetooh, sites não confiáveis...

### Componentes de um malware

| Termo | Descrição |
| ---- | ---- |
| Crypter | Software que protege o malware contra engenharia reversa ou análise |
| Downloader | Um tipo de Trojan que faz o download de outros malwares da Internet para o PC |
| Dropper | Um tipo de Trojan que instala secretamente outros arquivos de malware no sistema |
| Injector | Um programa que injeta seu código em outros processos em execução vulneráveis e altera a forma como eles são executados para esconder ou evitar sua remoção |
| Obfuscator | Um programa que oculta o código, tornando difícil para os mecanismos de segurança detectá-lo |
| Packer | Um programa que permite que todos os arquivos sejam agrupados em um único arquivo executável por meio de compressão, para evitar a detecção de software de segurança |
| Payload | Um pedaço de software que permite o controle sobre um sistema de computador após ter sido explorado |

### Tipos de malwares

- Trojan: se passa por um programa confiável
- Virus: um programa que cria cópias de si mesmo (**com** interação humana)
- Worm: um programa que cria cópias de si mesmo (**sem** interação humana)
- Ransomware: "sequestra" (bloqueia com criptografia) a máquina da vítima exigindo resgate
- Spyware: programa silencioso que monitora as atividades do usuário
- Keylogger: captura as teclas que estão sendo digitadas no teclado do usuário
- Botnets: uma máquina comprometida que realiza tarefas para o atacante
- Rootkits: programa que esconde a presença do atacante enquanto ele realiza as atividades maliciosas

| Virus | Worm |
| ---- | ---- |
| Infecta um sistema se anexando a um programa | Um worm infecta um sistema explorando uma vulnerabilidade no sistema, se replicando |
| Pode alterar arquivos | Geralmente não consegue alterar nada, só explorar a CPU e memória |
| Se espalha conforme programado | Espalha mais rápido |
| Só consegue se espalhar caso o arquivo infectado seja replicado para outros computadores | Depois de entrar no sistema, consegue espalhar de diversas maneiras |

## Vulnerabilidades

Vulnerabilidades são fraquezas que podem ser exploradas por conta de:
- **Má configuração**
- **Instalação padrão**
- **Buffer Overflows**
- **Servidores e sistemas operacionais desatualizados**
- **Credenciais padrão**
- **Falha na aplicação ou no design**
- **Zero-Day**

## Bônus

### Portas comuns utilizadas por trojans

![[Pasted image 20240111140612.png]]
