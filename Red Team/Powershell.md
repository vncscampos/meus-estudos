#techniques #techniques/privesc #techniques/privesc/windows #windows 

**Powershell** é uma linguagem de script do Windows e um ambiente shell feito com .NET. Por isso é possível executar comandos (cmdlets) de funções do .NET.

Os **cmdlets** são da forma `Verbo-Nome` por exemplo `Get-Command`

Lista de verbos comuns (mais [aqui](https://learn.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7.4&viewFallbackFrom=powershell-7)):
- Get
- Start
- Stop
- Read
- Write
- New
- Out

O `Get-Help` mostra informações sobre o cmdlet e o `Get-Command` lista todos os cmdlets.

```powershell
Get-Help Get-Command -Examples
Get-Command
Get-Command New-*
```

Assim como no shell do Linux também é possível usar pipeline para manipular o output.

Como o output é um objeto, diferente de outras shells que passam uma string para o próximo pipe, o Powershell passa um objeto. E como todo objeto (POO) ele possui métodos e propriedades.

```powershell
Get-ChildItem | Select-Object -Property Mode, Name
Get-Service | Where-Object -Property Status -eq Stopped
Get-ChildItem | Sort-Object
```

Lista de [operadores](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-6)

Procurar um arquivo

```powershell
Get-ChildItem -Path C:\ -Recurse -ErrorAction SilentlyContinue | Where {$_.Name -match 'nome'}
```

`pwd` -> `get-location`
`wget` -> `Invoke-WebRequest`

Procurar por API Keys

```powershell
Get-ChildItem -Path C:\Users -Recurse -ErrorAction SilentlyContinue | Select-String “API_KEY”
```

Ver conteúdo de backup

```powershell
Get-ChildItem “*.bak*” -Path C:\ -Recurse -ErrorAction SilentlyContinue | Get-Content
```
