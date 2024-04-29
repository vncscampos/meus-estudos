#attack #attack/web #web

**Serialização** é o processo de converter uma estrutura de dados, como um objeto, em uma cadeia de bytes sequencial para armazenar na memória, em um banco ou em um arquivo. Já a **desserialização** é o processo inverso.

A **desserialização insegura** ocorre quando os dados controlados pelo usuário são desserializados pelo site, permitindo o invasor manipular objetos serializados para injetar scripts. A desserialização da entrada do usuário deve ser evitada.

## Como identificar

Durante os testes é preciso olhar todos os dados que circulam no website e ver se algum deles parece serializado. É muito importante descobrir a linguagem utilizada e como ela serializa os dados.

O **Burp Scanner** (do professional :c) automaticamente alerta as mensagens HTTP que parecem conter objetos serializados.
### PHP

```PHP
$user->name = "carlos";
$user->isLoggedIn = true;
# Serializado
O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}
```

Os métodos nativos para a serialização em PHP são `serialize()` e `unserialize()`. Se você tem acesso ao código-fonte, deve começar procurando por `unserialize()` em qualquer lugar do código e investigando mais a fundo.

### Java

O Java utiliza serialização binária e para identificar que o objeto java foi serializado basta ver se os bytes começam com **"ac ed"** em hexadecimal ou **"rO0"** em Base64.

### Ruby

```ruby
User = Struct.new(:name, :role)
user = User.new('Mike', :admin)
puts Marshal.dump(user).inspect

"\x04\bS:\tUser\a:\tnameI\"\tMike\x06:\x06ET:\trole:\nadmin"
```

## Como explorar

### Modificar atributo dos objetos

```PHP
O:4:"User":2:{s:8:"username";s:6:"carlos";s:7:"isAdmin";b:0;}
# Modificar isAdmin -> 1
```

### Modificar os tipos dos dados

Lab: [[Insecure Deserialization - Modifying data types]]

### Usando funcionalidades da aplicação

Lab: [[Insecure Deserialization - Using application functionality]] 

### Magic methods

Métodos mágicos são métodos que não precisam ser explicitamente chamados. Eles são chamados automaticamente sempre que ocorre uma situação específica. 

Estes métodos podem ser adicionados a uma classe para determinar qual código deve ser executado quando um evento acontecer. Por exemplo, o construtor é um método sempre chamado quando um objeto da classe é instanciado, em PHP é chamado `__construct()` e em Python, `__init()__`.

Os métodos por si só não representam uma vulnerabilidade, porém, quando eles manipulam dados controlados pelo invasor, por exemplo, de um objeto serializado, isso pode ser explorado.

### Injetando objetos arbitrários

Na programação orientada a objetos, os métodos disponíveis para um objeto são determinados pela sua classe. Portanto, se um atacante puder manipular qual classe de objeto está sendo passada como dados serializados, eles podem influenciar qual código é executado após, e até durante, a desserialização.

Os métodos de desserialização geralmente não verificam o que estão desserializando. Isso significa que você pode passar objetos de qualquer classe serializável que esteja disponível para o site, e o objeto será desserializado. Isso permite efetivamente que um atacante crie instâncias de classes arbitrários.

Se um atacante tiver acesso ao código-fonte, ele pode estudar todas as classes disponíveis em detalhes. Para construir um exploit simples, eles procurariam por classes contendo **métodos mágicos** de desserialização, e então verificariam se algum deles realiza operações perigosas em dados controláveis. O atacante então pode passar um objeto serializado dessa classe para usar seu método mágico em um exploit.

Lab: [[Insecure Deserialiation - Arbitrary object injection PHP]]

### Gadget chains

Gadget são trechos de código que existe na aplicação. Ele sozinho não é um vetor de ataque, mas uma cadeia de gadgets pode ser um problema se um deles é inseguro e o invasor (que tem acesso ao input) invocar um método que passará para outro gadget.

#### pre-built

Identificar manualmente a cadeia de gadget é quase impossível sem acesso ao código-fonte, porém existem opções para trabalhar com cadeias pré construídas que você pode tentar primeiro.

- **ysoserial** -> java
- **phpggc**

### PHAR 

O **PHAR** é um arquivo que contêm metadados serialziados. Se você realizar qualquer operação de sistema de arquivos em um fluxo `phar://`, esse metadados serão desserializados implicitamente, portanto o `phar://` é um vetor para explorar *insecure deserialization*.

Esse é um caminha de exploração quando o website PHP não realiza a desserialização explícitamente.

Lab: [[Insecure Deserialization - PHAR]]