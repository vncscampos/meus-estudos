#soc #windows #endopoint_sec

Os arquivos de logs geralmente estão em `C:\Windows\System32\winevt\Logs` no formato binário .evt ou .evtx
## Event Viewer

Pode ser iniciado pelo menu Iniciar ou no prompt `eventvwr.msc`

![[Pasted image 20240417105509.png]]

O Visualizador de Eventos possui três painéis. O painel à esquerda fornece uma lista hierárquica em árvore dos provedores de logs de eventos. O painel central exibirá uma visão geral e resumo dos eventos específicos de um provedor selecionado. O painel à direita é o painel de ações.

## wevtutil.exe

O wevtutil é usado em linha de comando permitindo fazer filtros mais avançados.

```powershell
wevtutil /?
wevtutil qe Application /c:3 /rd:true /f:text #retorna 3 últimos eventos dos logs de application no formato de texto
```

## Get-WinEvent

```powershell
Get-WinEvent -List
Get-WinEvent -FilterHashtable @{ LogName='Application'; ProviderName='WLMS' }
```

## XPath Queries

O Windows Event Log permite visualizar detalhes de um evento em XML para realizar consultas avançadas e filtrar eventos com base em critérios específicos com o **XPath**.

![[Pasted image 20240418141614.png]]

- A primeira tag começa com `*` ou `Event` e é semelhante a  `Get-WinEvent -LogName Application -FilterXPath '*'`
- A proxima é `System` ->  `Get-WinEvent -LogName Application -FilterXPath '*/System/'`
- O EventID é 4798 ->  `Get-WinEvent -LogName Security -FilterXPath '*/System/EventID=4798'`

Com wevtutil ficaria

```powershell
wevtutil.exe qe Security /q:*/System[EventID=4798] /f:text /c:1
```

Exemplos:

```powershell
Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"] an
d */System/TimeCreated[@SystemTime="2020-12-15T01:09:08.940277500Z"]'


Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="System"' -MaxEvents 1

Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="Sam" and */System/EventID=4720' | Format-List *

Get-WinEvent -LogName System -FilterXPath '*/System/EventID=7045' -MaxEvent 1 | Fo
rmat-List *
```

