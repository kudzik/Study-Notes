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

### Podstawowe polecenia

`&&` - łączenie poleceń

`.` - aktualny katalog

`*` - wszystko lub dowolny znak

`^` - z wyłączeniem znaku

`>` lub `>>` - strumień danych (nadpisywanie, dodawanie)

> Nawigacja

```bash
# Katalog użytkownika
cd
cd ~

# Określony katalog
cd /etc/vim/

# Katalog nadrzędny
# cd ..
```

> Informacje

```bash
# Listowanie plików i katalogów
ls -l
# wszystkie pliki i katalogi
ls -la
ls -l /etc/vim

# Drzewo katalogów
# Może być konieczne doinstalowanie paczki
tree

# Informacje o pliku z metadanych
file nazwaPliku

# Wyświetlanie zawartości pliku
cat nazwaPliku

# dopisanie informacji do pliku
cat >> nazwaPliku
## kończymy CTRL+D

# łączenie plików i zapisanie informacji do nowego
cat plik1.txt plik2.txt > plik3.txt
```

> Filtrowanie informacji

```bash
cat /proc/cpuinfo

grep szukanaFraza /proc/cpuinfo
# Ignorowanie wielkości znaków

grep -i szukanaFraza /proc/cpuinfo
grep -i kudzik /etc/passwd

# Kilka fraz
grep -i "model\|core" /proc/cpuinfo

# filtrowanie folderów
grep -R szukanaFraza
```

> Tworzenie plików i katalogów

```bash
mkdir nowyKatalog
mkdir /home/download/nowyKatalog

# Nadawanie uprawnień podczas tworzenia
mkdir -m 775 nowyKatalog

# Tworzenie pliku
touch nazwaPliku

# plik ukryty ma kropkę przed nazwą
touch .nowyPlik

```

> Kopiowanie plików

```bash
cp ścieżka/plik.txt /lokalizacja

# zmiana nazwy pliku podczas kopiowania
cp ścieżka/plik.txt /lokalizacja/plikNowaNazwa.txt

# kopiowanie katalogu z całą zawartością
cp -r katalog /katalogDocelowy/
# Kopiowanie plików tylko zmodyfikowanych
cp -ru katalog /katalogDocelowy/
```

> Usuwanie plików i katalogów

```bash
rm nazwaPliku

# Katalog z zawartością
rm -r nazwaKatalogu

# Wszystkie pliki z katalogu
rm -r nazwaKatalogu/*

# Wszystkie pliki ze znakiem 9
rm -r nazwaKatalogu/*[9]

```

> Wyszukiwanie

```bash
# Wyszukiwanie po nazwie
find /lokalizacja -name -i "szukanaFraza"
find /home/kudzik/_Temp -iname "*.txt" -ls
```

> Porównywanie plików i katalogów

```bash
diff plik1.txt plik2.txt

# porównanie zawartości katalogów
diff -r katalog1 katalog2
```

> Aliasy

```bash
# Tworzenie nowego aliasu
alias update="sudo apt update"
update

# Lista dostępnych aliasów
alias
```

### Pipe

```bash
# wyświetl informacje > przefiltruj > zapisz do pliku
ip a | grep lo | cat > info.txt
```

### Zarządzanie użytkownikami

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

### Linki (dowiązania)

Linki dzielimy na `twarde` oraz `symboliczne`. Twarde tworzy nową nazwę dla zasobu i odwołuje śię do zawartości a nie do nazwy. Gdy usuniemy plik źródłowy link dowiązany pozostanie.

Dowiązanie symboliczne podobne są do skrótów z Windows po usunięciu pliku link symboliczny przestanie działać.

```bash
# Dowiązanie twarde
ln nazwaPliku.txt /katalog/plikLink.txt

# Dowiązanie symboliczne
ln -s nazwaPliku.txt /katalog/plikLink.txt
```

### Archiwizacja

Domyślnym archiwizatorem pod systemy linux jest `tar`. Tar domyślnie nie kompresuje plików, ale możliwe jest jej wymuszenie poprzez np. `gzip`.

`-f` występuje gdy pracujemy z nazwą

`-v` podgląd podczas tworzenia

```bash
# Tworzenie archiwum
tar -cf nazwaArchiwum.tar /katalog
# Dodanie pliku do istniejącego archiwum
tar -rf nazwaArchiwum.tar /katalog/plik

# podgląd
tar -tf nazwaArchiwum.tar

# Rozpakowanie archiwum
tar -xf nazwaArchiwum.tar
tar -xf nazwaArchiwum.tar -C /nowaLokalizacja

# Dodanie kompresji gzip
tar -cvzf nazwaArchiwum.tar.gz /katalog

# Kompresja istniejącego archiwum
gz nazwaArchiwum.tar
# Dekompresja
gunzip  nazwaArchiwum.tar.gz
```

## Administracja

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

## Zarządzanie dyskami i partycjami

`/dev` - od słowa device

Dyski w systemach linux oznaczane są za pomocą liter `sd` a następnie litera dysku `sda`, `sdb`, `sdc`.

Partycje na dyskach sa oznaczane kolejnymi liczbami `sda1`, `sda2`, `sdb1`, `sdb2`

```bash
# Informacje o dyskach
dmesg|grep sd
ls -l | grep sd
ls -l /dev/sd*

```

Popularne narzędzie służące do zarządzania partycjami  to `gdisk`

```bash
# Informacje
sudo gdisk /dev/sdb

# Tworzenie partycji
## +5g w last sector określi wielkość
## Zapis utworzonych partycji opcja w

```

> Formatowanie utworzonej partycji

```bash
sudo mkfs.ext4 /dev/sdb1
```

Po sformatowaniu partycji należy ją zamontować w systemie plików (podpiąć pod katalog).
O ile nie określimy inaczej montowanie trwa do momentu restartu systemu.

```bash
mkdir ~/pendrive
sudo mount /dev/sdb1 ~/pendrive

# Sprawdzenie montowania
 df -h | grep sd

 # Odmontowanie partycji
 sudo umount ~/pendrive
```

:bulb: konieczne może być nadanie uprawnień do dysku dla użytkowników

### LVM

`lsblk` - informacje o dyskach i partycjach

`Logical Volume Manager` - pozwala w łatwy sposób tworzyć i zarządzać partycjami, modyfikować ich wielkość i właściwości bez narażanie się na utratę danych. Dodatkowo w ramach utworzonej grupy możemy wydzielać partycje które będą większe niż fizyczny dysk.

W skład LVM wchodzą:

- woluminy fizyczne (fizyczny dysk lub partycja)
- grupa LVM (zbiory dysków lub partycji)
- wolumen logiczny LVM (partycja LVM wydzielona z grupy LVM)

:bulb: Utworzone partycje mogą być taką samą bazą dla wolumenu fizycznego jak podłączony dysk.

:exclamation: Konwersja partycji zniszczy zapisane na niej dane. Dysk w chwili konwersji nie może być zamontowany.

```bash
#Konwersja partycji na wolumin fizyczny
sudo pvcreate /dev/sdb1

# Tworzenie grupy woluminów
sudo vgcreate grupa1 /dev/sdb1 /dev/sdb2

# Wyświetlenie informacji o grupie
sudo vgdisplay

# Dodawanie nowych wolumenów do grupy
sudo vgextend grupa1 /dev/sdc

# po utworzeniu grupy można wydzielać na niej logiczne partycje
sudo lvcreate --name nazwaWoluminu --size 7g grupa1

# Maksymalny pozostały dostępny rozmiar grupy
sudo lvcreate --name reszta -l 100%FREE grupa1

# Formatowanie partycji
sudo mkfs.ext4 /dev/mapper/grupa1-samba

# montowanie partycji
sudo mount /dev/mapper/grupa1-nfs /mnt/nfs

# informacje
df -h

# Utworzone partycje możemy powiększać w ramach grupy pamiętając o powiększeniu systemu plików w jakim jest sformatowana.
sudo lvextend --size +1g --resizefs /dev/mapper/grupa1-nfs

# informacje
sudo lvs

```
Grupy LVM widoczne są w lokalizacji `/dev/mapper`

Automatyczne montowanie po starcie systemu `/etc/fstab`. Do montowania wykorzystuje się ID partycji.

```bash
# pobranie ID partycji
sudo blkid /dev/mapper/grupa1-nfs

# przełączamy się na root
sudo su

# Wpis ID do fstab za pomocą pipeline
blkid /dev/mapper/grupa1-nfs | cat >> /etc/fstab

Po edycji pliku format dodanych partycji powinien być następujący
# /dev/disk/by-uuid/c7b838f1-6c04-4f0d-b63d-91d0df15079c / ext4 defaults 0 1
# /swap.img       none    swap    sw      0       0
# UUID="c153bd94-4d05-4fbb-8999-7d6170eedc15" /mnt/nfs ext4
# UUID="219eb4bf-7c57-4a11-947e-1f555312ef1c" /mnt/reszta ext4
# UUID="4ce4ba9d-f651-416c-b753-7dbe913d4f64" /mnt/samba ext4

# Sprawdzenie po restarcie
df -h
```

## Serwery plików

### Samba

[Ubuntu tutorial](https://ubuntu.com/tutorials/install-and-configure-samba#1-overview)

`Server Message Block`

Umożliwia dzielenie się plikami pomiędzy maszynami windows i linux, w tym momencie jest to technologia Microsoftu. Na klientach nie jest wymagane żadne dodatkowe oprogramowanie. Protokół umożliwia również dostęp do AD a także serwera wydruku. Usługa działa w tle jako `deamon`.

Demon dostępu do plików to `smdb`.

> Instalacja pakietów

```bash
sudo apt install samba
```

Udostępnianie plików może być chronione hasłem lub dostęp może być publiczny. Jeśli chcemy udostępniać pliki które wymagają hasła koneczne będzie utworzenie nowego użytkownika. Login i hasło będą wykorzystane do uwierzytelnienia.

```bash
sudo adduser sambauser

# Dodanie użytkownika do samby i ustalenie hasła, może być inne niż hasło użytkownika.
sudo smbpasswd -a sambauser

# kopia zapasowa pliku
sudo cp /etc/samba/smb.conf /etc/samba/smb_old.conf
```

plik konfiguracji samby `/etc/samba`. Własną konfigurację możemy dodać na dole pliku.

```markdown
[dane]
   path = /mnt/reszta
   browseable = yes
   read only = no
[publiczny]
   path = /mnt/samba
   browseable = yes
   readonly = no
   guest ok = yes
```

Po zmianach należy zrestartować serwer samby

```bash
sudo service smbd restart

#Konieczne jest nadanie uprawnień do udostępnianych folderów
sudo chmod 777 /mnt/samba

# Status usługi
sudo systemctl status smbd

# Dostęp do plików z poziomu windows po IP serwera np.
# \\192.168.0.2
```