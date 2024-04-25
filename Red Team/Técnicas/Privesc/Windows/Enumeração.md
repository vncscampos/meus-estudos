#privesc #windows #enumeration #techniques 
## Network infrastructure

Primeira coisa a se fazer é entender a rede que está inserida

```powershell
> netstat -nao
```

O `netstat` vai mostrar todas as conexões TCP/UDP estabelecidas

```powershell
> ipconfig /all
> arp -a
```

O comando `arp` vai exibir a tabela ARP com IP e MAC das máquinas que estão se comunicando. O que ajuda a encontrar outras máquinas na rede e buscar por portas abertas.

## Usuários

```powershell
whoami
whoami /groups
net user
net group
net localgroup
net localgroup [nome do grupo]
net accounts /domain #vê se pertence a um dominio
```

## Active Directory environment

É um serviço de diretório baseado no Windows que armazena e fornece objetos de dados ao ambiente de rede interno. Como ele permite o gerenciamento centralizado da autenticação e autorização, ele contém detalhes de usuários como: nome, senha, cargo, grupos, permissões etc.

Componentes que a gente deve familiarizar com AD:

- Domain Controllers
- Organizational Units
- AD objects
- AD Domains
- Forest
- AD Service Accounts: Built-in local users, Domain users, Managed service accounts
- Domain Administrators

```powershell
> systeminfo
> wmic qfe get Caption,Description
```

### Gerenciamento de usuários e grupos 

O AD poder ter várias conta com permissões e funções diferentes:

- **built-in local user** -> usadas para gerenciar o sistema localmente, o que não faz parte do ambiente AD
- **domain user** -> acesso ao ambiente AD podendo usar os serviços
- **managed service** -> contas de domínio limitadas com privilégios elevadas para gerenciar os serviços do AD
- **administrators** -> gerencia as informações do AD, incluindo configurações, usuários, grupos, serviços, permissões etc

Contas de administradores do AD:

-  **BUILTIN\Administrator:** Acesso administrativo local em um controlador de domínio.
- **Domain Admins:** Acesso administrativo a todos os recursos no domínio.
- **Enterprise Admins:** Disponível apenas na raiz da floresta.
- **Schema Admins:** Capaz de modificar domínio/floresta; útil para membros da red team.
- **Server Operators:** Podem gerenciar servidores de domínio.
- **Account Operators:** Podem gerenciar usuários que não estão em grupos privilegiados.

```powershell
> Get-ADUser -Filter *
```

Pega todas as contas de usuários ativos do AD.

Também da para usar uma estrutura de árvore chamada LDAP. o `Distinguished Name` é uma coleção de pares de chave e valor separados por vírgula para achar registros dentro do AD. o DN consiste em:

- Domain Component (DC)
- OrganizationalUnitName (OU)
- Common Name (CN)

![[Pasted image 20240418202210.png]]

```powershell
> Get-ADUser -Filter * -SearchBase "CN=Users,DC=THMREDTEAM,DC=COM"
```

## Soluções de segurança em host

### Antivírus

```powershell
> Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct
```

### Windows Defender

```powershell
> Get-Service WinDefend
```

Também é possível ver as ameaças que o WinDefend identificou

```powershell
> Get-MpThreat
```

### Host-based firewall

```powershell
> Get-NetFirewallProfile | Format-Table Name, Enabled
```

Se tiver acesso aos privilégios do admin podemos tentar desativar

```powershell
> Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False
```

Ou ver as regras

```powershell
> Get-NetFirewallRule | select DisplayName, Enabled, Description
```

### Sysmon

Detectar se o Sysmon está instalado e rodando

```powershell
> Get-Process | Where-Object { $_.ProcessName -eq "Sysmon" }
> Get-CimInstance win32_service -Filter "Description = 'System Monitor service'"
> reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Channels\Microsoft-Windows-Sysmon/Operational
```

Depois de detectar, podemos tentar achar o arquivo de configuração e ver se temos permissão de ler para entender o que o sistema admin monitora.

```Powershell
> Get-ChildItem -Path C:\ -Recurse -Filter sysmonconfig.xml -ErrorAction SilentlyContinue
```

## Aplicações e serviços

Ver os aplicativos instalados e suas versões

```powershell
> wmic product get name,version
```

Procurar por diretórios escondidos

```powershell
> Get-ChildItem -Hidden -Path C:\Users\kkidd\Desktop\
```

Ver serviços iniciados:

```powershell
> net start
```

