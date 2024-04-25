#windows
## Arquitetura e operações do Windows

- API do Windows (Win32, COM, WinRT) traduz comandos do aplicativo (modo usuário) para o kernel
- Os comandos que o usuário chama para o kernel executar são conhecidas como **system calls**
- O kernel é o núcleo do SO e tem controle sobre todo o computador
- Camada de abstração de hardware (HAL) lida com a comunicação entre hardware e kernel
	- Há casos que o kernel comunica diretamente com hardware
	- O HAL também precisa do kernel para executar algumas funções

### Modo usuário

- Acessa recursos que o SO disponibiliza
	- Aplicativos só conseguem acessar o seu endereço privado que o SO deu
- Devido ao isolamento, as falhas de modo usuário são restritas apenas ao aplicativo e recuperáveis

### Modo kernel

- Acesso irrestrito ao hardware
- Não é isolado, se ocorre um erro no kernel o SO pode falhar
- Executa instruções de CPU
- Pode referenciar qualquer endereço da memória diretamente

## Sistema de arquivos

### exFAT

- Extended File Allocation Table
- Usado em dispositivos de armazenamento flash
- Tem limitações para número de partições, tamanho de partições e tamanho de arquivos

### HFS+

- Hierarchical File System Plus
- Usado no macOS
- O Windows consegue ler dados dessa partição com software

### EXT

- Extended Filesystem
- Usado no Linux
- O Windows também consegue ler dados dessa partição com software especial

### NTFS

- New Technology File System
- Suporta arquivos e partições muito grandes
- Oferece suporte de recuperações e de segurança
- Controle atividades do arquivo: MACE -> modify, access, create, entry modified

Antes de usar um HD por exemplo, ele deve ser formatado com um sistema de arquivos. A formatação do NTFS cria estruturas importantes no disco:

- **Setor de inicialização da partição**: 16 primeiros setores da unidade contendo MFT e 16 últimos contendo a copia do setor de inicialização
- **Master File Table:** tabela contém os locais de todos os arquivos e diretórios na partição, incluindo atributos de arquivo, como informações de segurança e carimbos de data/hora.
- **Arquivos de sistema:** arquivos ocultos com informações sobre outros volumes
- **Área do arquivo:** área principal da partição onde arquivos e diretórios são armazenados


## Inicialização do Windows

![[Pasted image 20240415221833.png]]

Quando ligado, o computador inicia a CPU para executar um programa na memória, com o código disponível em uma memória CMOS não volátil fornecida pelo fabricante, chamada *firmware*. Antigamente esse firmware era um programa chamado **BIOS** (Basic Input/Output System) e hoje em dia a maioria utiliza **UEFI** (Unified Extensible Firmware Interface). A sua função é iniciar o sistema operacional, carregando pequenos programas de inicialização que estão no início da partição de um disco.

Independente do firmware utilizado, no Windows o primeiro programa a ser chamado é o `Bootmgr`. Quando executado, ele verifica se o sistema estava hibernando ou no modo de espera. Se sim, ele carrega e executa o  `Winresume.exe`; do contrário carrega o `Winload.exe`.

O Winload carrega os componentes de inicialização para memória: o kernel (`ntoskrnl.exe`), o HAL (hal.dll) e System.

Outros processos principais também são carregados em seguida [[Core Windows Process]]