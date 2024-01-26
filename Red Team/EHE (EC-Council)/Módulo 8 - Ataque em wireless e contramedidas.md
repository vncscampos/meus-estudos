Wireless é uma rede WLANs baseada no padrão **IEEE 802.11**

## Criptografia de rede wireless

|  | WEP | WPA | WPA2 | WPA3 |
| ---- | ---- | ---- | ---- | ---- |
| Protocolo de criptografia | RC4 | TKIP | AES | AES |
| Método de autenticação | CRC | PSK | PSK | SAE |
| Tamanho da chave | 10,26 ou 32 dígitos hexadecimais | 8 a 64 caractéres | 8 a 64 caractéres | 8 a 64 caractéres |
| Nível de segurança | Fraca | Média | Alta | Muito alta |

## Ataques

### Rogue AP

O hacker pode criar um *access point* e induzir de alguma forma os usuários da rede se conectarem nele, podendo ser um AP próximo ao usuário (client mis-association) com SSID (nome da rede) chamativa "XYZ Corp Open Wi-fi".

### Má configuração do AP

Ao instalar um AP ele vem com uma conta padrão que na maioria das vezes pode ser `admin:admin` para acessar e configurar. Se essa conta não for alterada depois de configurar, qualquer usuário que estiver na rede wireless pode ter acesso às configurações do AP e até mesmo alterar.

### Conexão não autorizada

Uma das formas de um hacker entrar em uma rede interna, de uma empresa por exemplo, é enviar um malware para um vítima desta rede que ao executar, o PC dela vira um novo AP da rede, dando acesso ao hacker à rede da empresa.

### Conexão ad-hoc

As redes ad hoc são redes sem fio que dispensam o uso de um ponto de acesso comum aos computadores conectados a ela, de modo que todos os dispositivos da rede funcionam como se fossem um roteador, encaminhando comunitariamente informações que vêm de dispositivos vizinhos.

Esta vulnerabilidade permite o atacante a habilitar ad-hoc mode na rede, e por ela não possuir uma autenticação muito segura, o atacante consegue se conectar e comprometer a rede.

## Bluetooh

O Bluetooth é uma tecnologia de comunicação sem fio de curto alcance. Ele permite que telefones celulares, computadores e outros dispositivos troquem informações. Dois dispositivos habilitados para Bluetooth se conectam por meio de uma técnica de emparelhamento.

**Modos do bluetooh:**

- **Discoverable**:
	- Discoverable: fica visível para todos os dispositivos bluetooh habilitados
	- Limited discoverable: fica visível por um período
	- Non-discoverable: Não fica visível
- **Pairing:**
	- Non-pairable: rejeita requisições para parear
	- pairable: aceita requisições para parear e estabelece conexão

### Ataques

- **Bluesmacking:** semelhante ao ping da morte
- **Bluejacking:** Bluejacking é o uso do Bluetooth para enviar mensagens a usuários sem o consentimento do destinatário, semelhante ao envio de spams por e-mail
- **Bluesnarfing:** Bluesnarfing é um método de acesso a dados sensíveis em um dispositivo habilitado para Bluetooth. Um invasor dentro do alcance de um alvo pode usar software especializado para obter os dados armazenados no dispositivo da vítima. Para realizar o Bluesnarfing, um invasor explora uma vulnerabilidade no protocolo Object Exchange (OBEX) que o Bluetooth utiliza para trocar informações.
- **BlueSniff:** É útil para localizar dispositivos Bluetooth ocultos e discoverable.
- **Bluebugging:** invasor obtém acesso remoto a um dispositivo habilitado para Bluetooth de destino sem que a vítima esteja ciente
- **Blueprinting:** técnica para tentar descobrir o modelo do dispositivo
- **MAC Spoofing:** pega o MAC de um dispositivo com Bluetooh habilitado para roubar os pacotes enviados