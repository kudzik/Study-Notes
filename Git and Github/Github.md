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

Wygenerowany klucz dodajemy do swojego konta na [github](https://github.com/settings/keys). Repozytoria klonujemy poprzez zakładkę **SSH**

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

:bulb: Repozytorium zdalne jest często określane jako upstream repository.

## Pliki specjalne

`README.md` - główna strona repozytorium

`CODEOWNERS` - plik z danymi właścicieli kodu. Dzięki dodaniu pliku osoby które są w nim wymienione otrzymają automatyczne powiadomienie gdy zostanie wykonane żądanie `pull request`

```markdown
* @github_username
/docs/ @github_username
```

## Cechy repozytorium

- `Topics`: etykiety/tagi opisują tematycznie repozytorium, mogą również opisywać zastosowane technologie. Po kliknięciu możemy eksplorować tematycznie podobne repozytoria.
- `Issues`: planowanie/śledzenie pracy i błędów
- `Projects`: służą do zażądania pracą i zadaniami, jeśli mamy w projekcie stworzone `Issues` możemy również nimi zarządzać.
- `Insights`: Spostrzeżenia dają wgląd w informacje o projekcie, możemy sprawdzić szczegółowe informacje na jego temat.

## Role podczas współpracy nad projektem

- `Collaborators`: Osoby które pracują nad projektem, zwykle posiadają uprawnienia zo zarządzania kodem w głównej gałęzi.
- `Contributors` : Osoby pracujące nad kodem które mogą proponować zmiany i ulepszenia przy pomocy `pull request`, przeważnie posiadają mniejsze uprawnienia.

## Praca z gałęziami

Zmiany kodu takie jak np. dodanie nowej funkcjonalności powinny odbywać się na innej gałęzi niż główna. Po zakończeniu pracy nad nową funkcją dodatkową gałąź należy scalić z główną (najczęściej `master` lub `main`).

## Przepływ pracy

## Rozwidlenia

## Podstawowe polecenia

`git clone` - wykonuje pełną kopię zdalnego repozytorium do lokalnego folderu.

Przed wysłaniem plików zlecane jest pobranie zmian poprzez `git pull` i rozwiązanie konfliktów.

Możemy pobrać zmiany poprzez `git fetch` jeśli nie będzie żadnych konfliktów zostanie wykonany `fast-forwarded` następnie pobieramy zmiany za pomocą `git pull`

Jeśli pojawią się konflikty musimy je rozwiązać następnie dodać zmiany poprzez `git add`, wykonać commit poprzez `git commit` i dopiero wtedy wysłać zmiany do repozytorium poprzez `git push`
