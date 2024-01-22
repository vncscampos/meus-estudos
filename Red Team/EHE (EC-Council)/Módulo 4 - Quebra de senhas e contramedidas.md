
## Microsoft Authentication

### Security Account Manager

O SAM é o banco de dados local com as credenciais dos usuários do Windows. Este banco está em `c:\windows\system32\config\SAM`, porém é bloqueado durante execução então não é possível ler e nem copiar. Há algumas formas usada por invasores para extrair os hashes como por exemplo salvar o SAM nos registros do Windows ou então fazer uma cópia sombra do volume.

Uma conta no SAM é salvo como:

```
<user>:<id>:<LM>:<NTLM>
```

### NTLM

NT LAN Manager é um protocolo de autenticação que armazena as senhas dos usuários no SAM. Este protocolo existe 2 versões: NTLMv1 e NTLMv2. Embora a segunda versão seja para melhorar a segurança da primeira versão, o protocolo não é mais considerado muito seguro, mesmo presente em alguns sistemas.

### Kerberos

O Kerberos é o protocolo de autenticação utilizado e mais robusto. Ele proporciona uma autenticação mútua, ou seja, tanto o usuário quanto o servidor verificam a identidade um do outro e as mensagens que são trocadas cliente/servidor são protegidas contra *sniffing*.

O protocolo utiliza três componentes:
- Centro de distribuição de chaves - KDC
- Servidor de autenticação - AS
- Servidor de admissão de tickets - TGS

O processo funciona da seguinte forma:
1. O cliente solicita um ticket (TGT - Ticket Granting Ticket) para o AS
2. Recebido o TGT do AS o cliente apresenta ele para o TGS, que gera um outro ticket para acessar o serviço 
3. Acesso ao serviço

## Hash injection - Pass the Hash

O invasor captura o hash da senha do alvo e utiliza esse hash para se autenticar no sistema. Sistemas como NTLM aceitam os hashes como prova de autenticação

## LLMNR/NBT-NS Poisoning

O LLMNR (Link-Local Multicast Name Resolution) e o NBT-NS (NetBIOS Name Service) são dois protocolos de resolução de nomes usados em redes Windows. 

Em um ataque de envenenamento LLMNR/NBT-NS, um atacante tenta fornecer respostas falsas a requisições desses protocolos com o objetivo de enganar os dispositivos na rede para que aceitem informações de resolução de nome fraudulentas.

Por exemplo, quando um cliente tenta acessar um host desconhecido ou inválido, o servidor DNS não fornece uma resposta e ai vem o NBT-NS ou LLMNR realizando uma requisição broadcast para ver se alguem conhece esse host. O atacante responde a esta requisição se passando pelo host que está sendo procurado, assim o cliente passa suas credenciais como NTLMv2 hash para o hacker achando que está acessando o recurso certo.

## Pass the Ticket Attack

Neste ataque, o invasor captura o TGT válido de um usuário através da captura de tráfego de rede ou outras técnicas e utiliza esse ticket para acessar os serviços.

## Contramedidas

- Não utilizar como senha nome pessoal ou de parentes, data de aniversário etc
- Utilizar letras maiúsculas, minúsculas, números, caracteres especiais !#@
- Não compartilhar
- Trocar a senha regularmente
- Autenticação de dois fatores