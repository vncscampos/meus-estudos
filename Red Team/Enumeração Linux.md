#techniques #techniques/enum #techniques/enum/linux #linux

## Linux

### Sistema

```bash
ls /etc/*-release
cat /etc/os-release
hostname
```

```bash
cat /etc/passwd
cat /etc/shadow
cat /etc/group
ls -lh /var/mail/
```

```shell
dpkg -l
rpm -qa #rpm-based linux
```

### Usuário

```sh
who #vê quem esta logado
w   #vê quem está logado
last
```

```sh
whoami
id
```

#### Sudo

```sh
sudo -l
find / -perm -u=s -type f 2>/dev/null
```
### Rede

```sh
ip a
cat /etc/resolv.conf #DNS servers
```

```sh
netstat -tulnpa
```

```sh
sudo lsof -i
```

### Processos

```sh
ps aux
ps -ef
ps axjf
```
