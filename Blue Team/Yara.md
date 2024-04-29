#soc #cti 

**Yara** é usada para identificar padrões de texto e binários em arquivos, sendo utilizado para detectar padrões de *malwares*.

Para isso ela cria regras (rules), as regras consiste em um conjunto de strings e a condição que determina a lógica. Exemplo:

```C
rule hello_checker {
	strings:
		$hello = "Hello"
		$hello_upper = "HELLO"
		$hello_low = "hello"

	condition:
		any of them
}
```

https://github.com/InQuest/awesome-yara?tab=readme-ov-file#rules
## Cuckoo

O[ Cuckoo Sandbox](https://cuckoosandbox.org/) é um ambiente que executa um malware para testar as regras do Yara.

## Motores de escaneamento

### LOKI

- É o motor de escaneamento de assinaturas básico e original do YARA.
- Windows e Linux

### THOR

- Mais robusto e avançado
- Windows, Linux e macOS

### YAYA

- Ajuda pesquisadores a lidar com múltiplas regras YARA.
- Linux

## [yarnGen](https://github.com/Neo23x0/yarGen)

Gerador de yara rules