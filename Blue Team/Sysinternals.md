#soc #endpoint #endpoint/sec #windows

O **Sysinternals** é uma coleção de utilitários de sistema para diagnosticar e solucionar problemas no Windows.

## Sigcheck

E um utilitário que funciona em linha de comando para verificar o número da versão de um arquivo, informações de timestamp, assinaturas, certificados.

Possui integração com VirusTotal para verificar o arquivo pesquisado.

```powershell
> sigcheck -u C:\Users\Admin\Desktop\file.txt
```

## Streams

O NTFS armazena um "compartimento secreto" em um arquivo chamado **Alternate Data Streams** (ADS). O ADS pode conter informações adicionais e não é exibido pelo Windows Explorer.

Malwares têm usado ADS para ocultar dados em um endpoint.

Com o **Streams** do Sysinternals é possível ver se há um fluxo alternativo presente.

```powershell
> streams C:\Users\Administrator\Desktop\file.txt
```

## SDelete

Essa ferramenta permite excluir de forma segura arquivos e espaço livre em disco, tornando praticamente irrecuperáveis.

```powershell
> sdelete -p 3 -s -q caminho_do_arquivo #para excluir um arquivo de forma segura
> sdelete -p 3 -z unidade: #para excluir uma unidade
```

- -p 0 -> passagem rápida sobrescrevendo com zeros
- -p 1 -> uma passagem sobrescrevendo com zeros
- -p 3 -> três passagens sobrescrevendo de forma aleatória
- -p 7 -> sete passagens sobrescrevendo de forma aleatória

## TCPView

Ferramenta que lista todas as conexões TCP e UDP do sistema, incluindo os endereços, as portas, o estado da conexão.

```powershell
> tcpview -accepteula
```

## Autoruns

Mostra os programas que estão configurados para executar durante a inicialização. É uma boa ferramenta para verificar malwares persistentes.

```powershell
> autoruns
```

## ProcDump

Uma utilidade de linha de comando para monitor um aplicativo em busca de picos de CPU.

```powershell
> procdump -accepteula
```

## Process Explorer

Mostra uma lista de processos ativos na janela superior e detalhes do processo selecionado na tabela de baixo.

Algumas opções são interessantes de incluir: **Verifiy Signatures**, **Run at  Logon** e **Replace Task Manager**.

Significado das cores dos processos:

- **Roxo:** indica que os arquivos podem ser empacotados
- **Vermelho:** o processo está parando
- **Verde:** foi gerado recentemente
- **Azul claro:** processo está rodando na mesma conta que abriu o Process Explorer
- **Rosa:** o processo é um serviço (por exemplo svchost.exe)
- **Cinza escuro:** processo suspenso ate clicar em "Resume" 

## Process Monitor

É uma ferramenta avançada de monitoramento que mostra atividades em tempo real do sistema de arquivos, registros e processos/threads.

```powershell
> procmon -accepteula
```

## PsExec

Permite executar processos em outros sistemas, com total interatividades para aplicativos de console.

## WinObj

É um programa que utiliza a API nativa do Windows NT (NTDLL.DLL) para acessar e exibir informações sobre o espaço de nomes do Gerenciador de Objetos NT.

O Gerenciador de objetos é uma estrutura que organiza os objetos em uma hierarquia semelhante a uma árvore. Cada nó é um objeto e esses objetos são processos, arquivos, chaves de registro e outros recursos.

```powershell
> winobj -accepteula
```