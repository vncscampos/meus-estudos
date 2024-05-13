#techniques #techniques/privesc #techniques/privesc/windows #windows 

Username: jasmine.stanley Password: G0O6Zd5aM

## Criando processos remotamente

### PSexec

- Porta 445/TCP (SMB)
- Requer um membro do grupo Administrators

O PSexec  funciona da seguinte forma:

1. Envia um serviço executável para ADMIN$ através do SMB
2. Conecta com SCM para rodar esse serviço
3. Comunicação através de [[Named Pipe]]

```powershell
psexec64.exe \\[IP] -u [usuario] -p [senha] -i cmd.exe
```

### WinRM

- Porta 5985/TCP (WinRM HTTP) ou 5986/TCP (WinRM HTTPS)
- Requer um membro do grupo Remote Management Users

A maioria dos Windows Server possui habilitado por padrão o **Windows Remote Management** que é um protocolo baseado em web que permite enviar comandos Powershell para um máquina remota.

```powershell
winrs.exe -u:[usuario] -p:[senha] -r:[alvo] cmd
```

ou

```powershell
$username = "vncs";
$password = "123";
$securePassword = ```powershell
ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
Enter-PSSession -Computername [algvo] -Credential $credential
Invoke-Command -Computername [alvo] -Credential $credential -ScriptBlock {whoami}
```

### sc.exe

- 135/TCP, 49152-65535/TCP (DCE/RPC)
- 445/TCP (RPC over SMB Named Pipes)
- 139/TCP (RPC over SMB Named Pipes)
- Requer membro do grupo Administrators