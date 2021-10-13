# Ubuntu Server

## Podstawowe informacje

### Pakiety

W dystrybucjach opartych na debianie domyślnym ich managerem jest `apt`.

```bash
# Aktualizacja źródeł pakietów
sudo apt update

# Aktualizacja pakietów
sudo apt upgrade

# Wyświetlenie listy zainstalowanych pakietów
apt list --installed

# Instalacja pakietu
sudo apt install examplePackageName
```

> Operatory

`&&` - łączenie poleceń

> Użytkownicy

```bash
# zmiana użytkownika na root
sudo su
exit

# zmiana użytkownika na kudzik
su kudzik

# utworzenie użytkownika
sudo adduser artur

# Dodanie użytkownika do grupy podczas tworzenia
sudo adduser exampleUsername --ingroup sudo

# Dodanie użytkownika do grupy
sudo usermod -a -G <exampleGroup> <exampleUsername>
sudo usermod -aG sudo <userName>

# Zmiana hasła dla użytkownika
## W ubuntu po zmianie hasła dla root będzie można się na niego zalogować
passwd exampleUsername

# Modyfikacja użytkownika
sudo usermod <userName>

# Usunięcie użytkownika
deluser [options] [--force] [--remove-home] [--remove-all-files] [--backup] [--backup-to DIR] <userName>
sudo deluser <userName>
```

- Dodać użytkownika do grupy możemy poprzez edycję pliku `/etc/group`, kolejnych użytkowników dodajemy po przecinku.
- Plik z użytkownikami i konfiguracją `/etc/passwd`
- Plik z hasłami `/etc/shadow`
- Plik z konfiguracją usuwania użytkowników `/etc/deluser.conf`

### Uprawnienia do plików

| Odczyt | Zapis | Wykonywanie |
| :----: | :---: | :---------: |
|   r    |   w   |      x      |
|   4    |   2   |      1      |

Oznaczenie dostępu do plików i katalogów`rwxrwxr-x` lub katalog `drwxr-xr-x`

| Czy katalog | Użytkownik | Grupa | Pozostali |
| ----------- | ---------- | ----- | --------- |
| -           | rwx        | rwx   | r-x       |
| d           | rwx        | r-x   | r-x       |

```bash
# Dodaj do istniejących uprawnień możliwość zapisu
# (o - others, a - all, u - user, g - group)
chmod o+w <catalogOrFile>
# Usuń z istniejących uprawnień możliwość zapisu
chmod o-w <catalogOrFile>
chmod o-rwx <catalogOrFile>

# Dodanie prawa dla wykonywania
chmod +x <catalogOrFile>

```

W zapisie liczbowym sumujemy liczby z każdej kategorii i uprawnienia są wykonywane od prawej do lewej.

```bash
# pozostali (4+1 odczyt + wykonanie)
# grupa (4+2 odczyt + zapis)
# właściciel (4+2+1 odczyt + zapis + wykonanie)
chmod 765 <catalogOrFile>

# Możemy zastosować `0` jeśli nie chcemy nadawać żadnych uprawnień
chmod 760 <catalogOrFile>
```

Zmiana właściciela pliku lub katalogu

```bash
sudo chown <NewUserName> <catalogOrFile>
```

:bulb: Jeśli chcemy aby uprawnienia do katalogu mało kilka osób ale nie mogli usuwać nie swoich plików możemy zastosować `sticky bit`.

```bash
chmod 777 <catalog>
chmod a+t <catalog>
# lub w notacji cyfrowej
chmod 1777 <catalog>
```

W uprawnieniach na końcu pojawi się litera t `drwxrwxrwt`

### Konfiguracja sieci

Pliki konfiguracji sieci znajdują się w katalogu `/etc/netplan`

```bash
# Sprawdzenie interfejsów sieciowych oraz IP
ip a
```

```yml
# This is the network config written by 'subiquity'
network:
  version: 2
  wifis:
    wlx74da38620205:
      access-points:
        UPC1234567:
          password: <WifiPassword>
      dhcp4: true
```

Po dodawaniu sieci lub aktualizacji pliku zmiany zatwierdzamy przy pomocy

```bash
sudo netplan apply
```

### SSH konfiguracja

```bash
# Sprawdzenie statusu usługi ssh
sudo systemctl status ssh
# Restart
sudo systemctl restart ssh

# Instalacja
sudo apt install openssh-server
```

Ustawienie prostego firewall i odblokowanie portu tylko dla SSH

```shell
ufw allow OpenSSH
ufw enable
ufw status
```

## Dodawanie użytkownika

Dodanie użytkownika i nadanie uprawnień do polecenia sudo
