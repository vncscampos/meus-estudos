#soc #endpoint #endpoint/sec #windows

O **Sysmon** reúne logs detalhados e de alta qualidade, bem como rastreamento de eventos que ajudam a identificar anomalias em seu ambiente. O Sysmon é mais comumente usado em conjunto com sistemas de gerenciamento de informações e eventos de segurança (SIEM), em um cenário ideal, os eventos seriam encaminhados para um SIEM para análise adicional.

Todos os eventos armazenados pelo sysmon está em `Applications and Services Logs/Microsoft/Windows/Sysmon/Operational`.

O Sysmon precisa de um arquivo de configuração que pode ser criado ou baixado como por exemplo o arquivo criado pelo SwiftOnSecurity: [Sysmon-Config](https://github.com/SwiftOnSecurity/sysmon-config).

## Eventos importantes

### Event ID 1: Criação de processo

O código abaixo exclui eventos quando `svchost.exe` cria um processo.

```xml
<RuleGroup name="" groupRelation="or">  
  <ProcessCreate onmatch="exclude">  
    <CommandLine condition="is">C:\Windows\system32\svchost.exe -k appmodel -p -s camsvc</CommandLine> 
  </ProcessCreate>
  <ProcessAccess onmatch="include">  
<TargetImage condition="image">lsass.exe</TargetImage>  
</ProcessAccess>
</RuleGroup>
```

### Event ID 3: Conexão de rede

Usado para detectar conexões remotas. O código abaixo cria eventos de ação do `nmap.exe` e identifica a porta 4444 aberta que é uma porta comum do Metasploit. 

```xml
<RuleGroup name="" groupRelation="or">  
  <NetworkConnect onmatch="include">  
    <Image condition="image">nmap.exe</Image>  
    <DestinationPort name="Alert,Metasploit" condition="is">4444</DestinationPort> 
  </NetworkConnect>  
</RuleGroup
```

```powershell
> Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"]=4444'
```

Para detectar outros malwares basta trocar a porta. Ao excluir uma porta é importante ficar atento a isto pois essa porta excluída pode ser alvo para malwares.
### Event ID 7: Image Loaded

Investiga DLLs carregados por processos que pode ser útil quando está procurando por DLL injection ou DLL hijacking. O código abaixo cria evento quando um DLLs é carregado em `\Temp\` o que pode ser considerado estranho.

```xml
<RuleGroup name="" groupRelation="or">  
<ImageLoad onmatch="include">  
  <ImageLoaded condition="contains">\Temp\</ImageLoaded>  
</ImageLoad>  
</RuleGroup>
```

### Event ID 8: CreateRemoteThread

O `CreateRemoteThread` monitora processos injetando código em outros processos legítimos mas pode ser usado por *malware* para se esconder. O código abaixo verifica o endereço de memória em busca de uma condição específica que pode indicar um beacon **Cobalt Strike** e também alerta quando acha processos injetados que não possuem pai.

```xml
<RuleGroup name="" groupRelation="or">  
<CreateRemoteThread onmatch="include">  
  <StartAddress name="Alert,Cobalt Strike" condition="end with">0B80</StartAddress>  
  <SourceImage condition="contains">\</SourceImage>  
</CreateRemoteThread>  
</RuleGroup>
```

```powerhshell
> Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=8'
```
### Event ID 11: Arquivo criado

Log eventos quando arquivos são criados ou sobrescritos no endpoint. O trecho de código é um exemplo de um monitor de eventos de ransomware. Este é apenas um exemplo de uma variedade de maneiras diferentes de utilizar o Evento ID 11.

```xml
<RuleGroup name="" groupRelation="or">  
<FileCreate onmatch="include">  
  <TargetFilename name="Alert,Ransomware" condition="contains">HELP_TO_SAVE_FILES</TargetFilename>  
</FileCreate>  
</RuleGroup>
```

### Event ID 12/ 13/ 14: Evento de registro

Busca por alterações no registro que pode indicar atividades maliciosas que inclui persistência ou abuso de credencial.

```xml
<RuleGroup name="" groupRelation="or">  
<RegistryEvent onmatch="include">  
  <TargetObject name="T1484" condition="contains">Windows\System\Scripts</TargetObject>  
</RegistryEvent>  
</RuleGroup>
```

### Event ID 15: FileCreateStreamHash

Procura por qualquer criação de arquivo em ADS.

```xml
<RuleGroup name="" groupRelation="or">  
<FileCreateStreamHash onmatch="include">
  <TargetFilename condition="contains">Downloads</TargetFilename>  
  <TargetFilename condition="contains">Temp\7z</TargetFilename>
  <TargetFilename condition="end with">.hta</TargetFilename>
  <TargetFilename condition="ends with">.bat</TargetFilename>
</FileCreateStreamHash>  
</RuleGroup>
```

### Event ID 22: Evento DNS

Faz log de todas queries DNS realizadas. É comum excluir eventos de domínios confiáveis que são considerados "barulho" (noise).

```xml
<RuleGroup name="" groupRelation="or">  
<DnsQuery onmatch="exclude">  
  <QueryName condition="end with">.microsoft.com</QueryName>  
</DnsQuery>  
</RuleGroup>
```


## Melhores práticas

- Exclude > Include
- CLI fica menos necessário
- Conheça o ambiente antes da implementação

## Detectando mimikatz

O método mais comum é ver quando um processo executa o **LSASS**. Como o `svchost.exe` executa o LSASS a gente exclui ele para não gerar "noise".

```xml
<RuleGroup name="" groupRelation="or">  
<ProcessAccess onmatch="exclude">  
<SourceImage condition="image">svchost.exe</SourceImage>  
</ProcessAccess>  
<ProcessAccess onmatch="include">  
<TargetImage condition="image">lsass.exe</TargetImage>  
</ProcessAccess>  
</RuleGroup>
```

```powershell
Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=10 and */EventData/Data[@Name="TargetImage"] and */EventData/Data="C:\Windows\system32\lsass.exe"'
```

