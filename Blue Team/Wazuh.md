#soc #network #endpoint #endpoint/sec

- O Wazuh é uma EDR
- Composto por duas partes:
	- Agente: quem vai ser monitorado
	- Manager: quem gerencia as atividades
- Consegue fazer scan de aplicações instalados e suas versões
- Calcula o score de um agente em relação a frameworks (NIST, MITRE, GDPR)
- Consegue colher logs do Sysmon:
	- `C:\> Sysmon64.exe -accepteula -i detect_powershell.xml`
	- Adicionar a regra para capturar esse evento no agent (C:\\Program Files (x86)\\ossec-agent\\ossec.conf) e no manager (/var/ossec/etc/rules/local_rules.xml)
- Possui uma API para fazer pesquisas `https://10.10.237.216:55000/manager`
- Gera reports:
	- 1. Modules
	- 2. Security Events
	- 3. Generate report
	- 4. Wazuh > Management > Reporting