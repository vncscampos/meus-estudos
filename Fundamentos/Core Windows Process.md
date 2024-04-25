#fundamentals #windows

## System

O PID para um processo é sempre aleatório exceto para o processo *System* que é sempre 4. Ele é um tipo especial de thread que opera apenas no modo kernel. Elas usam espaço de endereçamento especial da memória chamada de **pool**, que podem ser paginados ou não.

O `ntosknrl.exe` que é o núcleo do Windows gerencia e executa as *threads* de sistema que são executadas sob o processo System.

Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai diferente do System Idle Process (0).
2. Múltiplas instâncias do System. Normalmente, deve haver apenas uma instância.
3. Um PID diferente de 4. Normalmente, o PID será sempre 4.
4. Não estar em execução na Sessão 0. Este processo normalmente é executado na Sessão 0.
## smss.exe

O **Session Manager Subsytem** é responsável por criar novas sessões e gerenciá-las. É o primeiro processo de modo usuário iniciado pelo kernel.

Ela inicia o modo kernel (win32k.sys) e o modo usuário (winsrv.dll e csrss.exe) e qualquer outro subsistema com valor `Required` em `HKLM\System\CurrentControlSet\Control\Session Manager\SubSystems`.

O csrss.exe e wininit.exe inicia na Sessão 0, uma sessão para o sistema operacional e o winlogon.exe e csrss.exe  na Sessão 1, que é a sessão do usuário.

Ele também é responsável por:
- Criar variáveis de ambiente
- Iniciar o `winlogon.exe`
- Paginação de memória virtual

Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai diferente do System (4).
2. O caminho da imagem é diferente de `C:\Windows\System32`.
3. Mais de um processo em execução. Normalmente, as instâncias filhas se encerram após cada nova sessão.
4. O usuário em execução não é o usuário SYSTEM.
5. Entradas de registro inesperadas para o Subsistema.
## csrss.exe

O **Client Server Runtime Process** é um processo crítico do lado do usuário, responsável por:
- janela do console win32
- criação e exclusão de threads de processos
- deixar a API do Windows disponível para outros processos
- lidar com processo de desligamento
  
Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai real. Normalmente, o processo `smss.exe` chama esse processo e se encerra.
2. O caminho do arquivo de imagem é diferente de `C:\Windows\System32`.
3. Ortografia sutil errada para ocultar processos maliciosos se passando por csrss.exe à primeira vista.
4. O usuário em execução não é o usuário SYSTEM.

## wininit.exe

O **Windows Initialization Process** é um processo crítico que roda em background responsável por lançar o `services.exe`, `lsass.exe` e `lsaiso.exe` na sessão 0.

Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai real. Normalmente, o processo `smss.exe` chama esse processo e se encerra.
2. O caminho do arquivo de imagem é diferente de `C:\Windows\System32.
3. Ortografia sutil errada para ocultar processos maliciosos à primeira vista.
4. Múltiplas instâncias em execução simultaneamente.
5. Não está sendo executado como o usuário SYSTEM.

### services.exe

O **Service Control Manager** é responsável por lidar com serviços do sistema: carregar serviços, interagir com eles e iniciar ou encerra-los.

E o que é serviço? É um processo iniciado pela máquina que roda em background e realiza funções como gerenciamento de rede, atualização do sistema etc.

Informações de serviços são armazenados em `HKLM\System\CurrentControleSet\Services`

Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai diferente de wininit.exe.
2. O caminho do arquivo de imagem é diferente de `C:\Windows\System32`.
3. Ortografia sutil errada para ocultar processos maliciosos à primeira vista.
4. Múltiplas instâncias em execução simultaneamente.
5. Não está sendo executado como o usuário SYSTEM.

#### svchost.exe

O **Service Host** é responsável por hospedar e gerenciar grupos de serviços que são implementados com DDLs.

As DLLs (Dynamic Link Libraries) são arquivos com códigos e dados que podem ser utilizados por mais de um programa ao mesmo tempo. São carregadas dinamicamente na memória durante a execução de um programa.

Por existir vários processos svchos.exe em qualquer Windows, ele é alvo para ataques maliciosos por exemplo criando um malware com esse nome ou um DLL malicioso que é chamado pelo svchost.exe legítimo.

Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai diferente de services.exe.
2. O caminho do arquivo de imagem é diferente de `C:\Windows\System32`.
3. Ortografia sutil errada para ocultar processos maliciosos à primeira vista.
4. A ausência do parâmetro -k. Este parâmetro é comumente usado pelo svchost.exe para especificar o grupo de serviços que está hospedando. Sua ausência pode ser incomum.

### lsass.exe

O **Local Security Authority Subsystem Service** é o processo responsável pelas políticas de segurança do sistema. Verifica os usuários logados, cria tokens de acesso para SAM, AD e NETLOGON e lida com as alterações de senha.

É um alvo para dump de credenciais com **mimikatz**.

Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai diferente de wininit.exe.
2. O caminho do arquivo de imagem é diferente de `C:\Windows\System32`.
3. Ortografia sutil errada para ocultar processos maliciosos à primeira vista.
4. Múltiplas instâncias em execução simultaneamente.
5. Não está sendo executado como o usuário SYSTEM.

## winlogon.exe

Responsável por carregar o perfil do usuário. Ele carregar o `NTUSER.DAT` em `HKCU` e `userinit.exe` que carrega o shell do usuário.

  
Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai real. Normalmente, o processo `smss.exe` chama esse processo e se encerra.
2. O caminho do arquivo de imagem é diferente de `C:\Windows\System32`.
3. Ortografia sutil errada para ocultar processos maliciosos à primeira vista.
4. Não está sendo executado como o usuário SYSTEM.
5. O valor do shell no registro é diferente de explorer.exe.

## explorer.exe

É o processo que da ao usuário acesso aos diretórios e arquivos e outras coisas como Menu Iniciar e Barra de Tarefas.

  
Para esse processo, comportamentos incomuns podem incluir:

1. Um processo pai real. Normalmente, o processo `userinit.exe` chama esse processo e se encerra.
2. O caminho do arquivo de imagem é diferente de `C:\Windows`.
3. Está sendo executado como um usuário desconhecido.
4. Ortografia sutil errada para ocultar processos maliciosos à primeira vista.
5. Conexões TCP/IP de saída. Isso pode ser incomum dependendo da natureza do processo e do sistema em questão.

