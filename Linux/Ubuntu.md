# Ubuntu Server

## Adres IP

```bash
ip a
```

Ustawienie prostego firewall i odblokowanie portu tylko dla SSH

```shell
ufw allow OpenSSH
ufw enable
ufw status
```

## Dodawanie użytkownika

Dodanie użytkownika i nadanie uprawnień do polecenia sudo

```bash
add user <username>
usermod -aG sudo
```