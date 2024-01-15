
## Sniffing

Sniffing é "xeretar" a rede, ou seja, capturar e monitorar todo pacote que está trafegando, colocando a interface de rede ou NIC, em modo promíscuo. Isso porque por padrão, o pacote que não é para máquina é ignorado, mas colocando neste modo, o pacote é capturado.

O sniffing pode ser passivo, quando um pacote chega até você sem precisar ter feito nada, como por exemplo em uma rede que é usado um **hub**. Ou ativo, quando o sniff é feito em redes com **switch** e por conta disso é necessário usar algumas técnicas como:

| Técnica | Resumo |
| ---- | ---- |
| **ARP Spoofing** | - ARP é o protocolo que link o endereço IP à um endereço físico da máquina (MAC)<br>- Um pacote ARP é mandado para switch quando um host quer mandar uma mensagem para um IP mas não conhece o MAC<br>- O switch quando não tem anotado de qual MAC é aquele IP ele manda para todos da rede perguntando quem é<br>- O hacker recebe essa informação e fala que é ele e assim, a vítima manda mensagem para o hacker. |
| **DNS Poisoning** | - O servidor DNS possui uma tabela que associa um domínio à um IP<br>- O atacante altera o IP de um domínio e quando uma vítima acessar esse domínio, será redirecionado para o IP do atacante |
| **MAC Flooding** | - O ataque envolve encher a tabela CAM (tabela que associa o MAC à porta) de MACs falsos, dessa forma o switch vai funcionar como um hub |
| **DHCP Starvation** | - Neste ataque, várias requisições DHCP são mandadas para o servidor DHCP com IPs falsos, esgotando o número de IPs disponíveis para rede (DoS) |
| **MAC Duplicating** | - Existe situações que apenas MAC autorizado podem acessar um segmento de rede<br>- Neste caso, o hacker rouba o MAC de um usuário legítimo para conseguir acesso à este segmento |

Existem alguns protocolos que são vulneráveis a sniff por não criptografar os dados trafegados como Telnet, IMAP, HTTP, FTP, POP, SMTP e NNTP.

### Mitigação

- Restringir o acesso físico ao meio de rede
- Usa criptografia ponta a ponta
- Usar switch em vez do hub
- Usar ferramentas que permite verificar se há NICs rodando no modo promíscuo
- Usar ACL
- Usar mecanismo de filtragem em para MAC
- Recupere os endereços MAC diretamente das placas de rede (NICs) em vez do sistema operacional; isso evita a falsificação de endereços MAC.

## Negação de serviço

- DoS: O hacker flooda o sistema da vítima com requisições acima do suportado interrompendo-a (1->1)
- DDoS: Ataque coordenado com múltiplas máquinas comprometidas (botnet) atacando um alvo para interromper o serviço (N->1)

**Técnicas:**
- UDP Flood
- ICMP Flood
- Ping da morte
- Smurf Attack -> manda várias requisições ICMP ECHO para uma rede IP broadcast so que com IP de origem alterado (IP da vitima), assim a resposta do ICMP vai para vítima sobrecarregando ela
- SYN Flood
- Multi vetor (sequencial e paralelo)
- Peer-to-peer

### Mitigação

- Habilitar TCP SYN cookie
- Configurar firewall
- Usar IDS
- Atualizar o sistema e desabilitar serviços inseguros ou não usados
- Usar ferramentas como Cloudflare, Imperva, F5, Ati DDoS Guardian etc

## Session Hijacking

- O hacker rouba um comunicação TCP válida entre dois computadores, no início desta comunicação
- Com isso o hacker pode controlar todo tráfego dessa sessão TCP

### Mitigação

- IDS
- Implementar logout no fim de uma sessão
- SSH
- Usar sistemas de autenticação fortes como Kerberos
- VPN
- SSL