# Github

- GitHub to usługa hostingowa dla GIT
- Oferuje dodatkowe narzędzia do zarządzania projektem
- Zapewnia narzędzia wykorzystywane podczas wspólnej pracy z kodem

> Główne cechy GitHub:

- zarządzanie kodem
- pull request (żądanie przeniesienia kodu do innej gałęzi)
- wspólna praca z problemami w kodzie
- CI/CD

## Połączenie za pomocą ssh

Klucze znajdują się w katalogu użytkownika w folderze `.ssh`. Jeśli katalog jest pusty lub nie istnieje konieczne jest ich wygenerowanie.

Windows oraz Linux:
```bash
ssh-keygen
```
Klucz znajdziemy w `.ssh/id_rsa.pub`

Wygenerowany klucz dodajemy do swojego konta na [github](https://github.com/settings/keys). Repozytoria klonujemy poprzez opcje **SSH**

```bash
git clone git@github.com:kudzik/<repo_name>.git
```

## Wyszukiwanie repozytoriów
- Wyszukiwanie repozytoriów
- Wyszukiwanie wewnątrz repozytorium
- [Advanced search](https://github.com/search/advanced)

## Praca z repozytoriami

Repozytoria mogą być postrzegane jako foldery dla projektów i mogą być tworzone na dwa sposoby:

- Możemy stworzyć repozytorium na **github** a następnie sklonować go na lokalny komputer
- Możemy stworzyć repozytorium lokalnie a następnie dodać do niego repozytorium zdalne (origin) i wysłać go na **github**

Repozytoria na **github** mogą być

- publiczne:
Każdy użytkownik może przeglądać, ale zmiany mogą wprowadzać tylko określone osoby

- prywatne:
Tylko określeni użytkownicy mają dostęp do jego przeglądania lub wprowadzania zmian.

