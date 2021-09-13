# GIT NOTE

## Linki

[Podstawowe polecenia](https://training.github.com/downloads/pl/github-git-cheat-sheet/)

[GIT Manual](https://git-scm.com/book/en/v2)

[GIT Manual PL](https://git-scm.com/book/pl/v2)

[GIT Aliases](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/git/README.md#aliases)

[GIT School Visualizing](https://git-school.github.io/visualizing-git/#free)

[Przykładowe pliki .gitignore](https://github.com/github/gitignore)

## Pomoc

```bash
git help <verb>
git <verb> --help
man git-<verb>
```

### Czym jest GIT

Git jest systemem kontroli wersji *VCS*  (Version Control System), umożliwia śledzenie wprowadzonych zmian w plikach w czasie.

### Cechy

- Migawki, nie różnice
- Niemal każda operacja jest lokalna
- Git ma wbudowane mechanizmy spójności danych (SHA-1)
- Standardowo Git wyłącznie dodaje nowe dane

### Możliwości

- Cofanie zmian w pliku lub całego projektu.
- Umożliwia podgląd zmian które zostały wprowadzone na przestrzeni czasu.
- Umożliwia współpracę i podgląd kto wprowadził zmiany.
- Umożliwia podgląd powodu wprowadzonej zmiany.
- Może być wykorzystany dla dowolnego typu pliku, nie tylko dla kodu źródłowego.

### Zalety

- Szybkość
- Prosta konstrukcja
- Silne wsparcie dla nieliniowego rozwoju (tysięcy równoległych gałęzi)
- Pełne rozproszenie
- Wydajna obsługa dużych projektów, takich jak jądro Linuksa (szybkość i rozmiar danych)

## Wady

- Brak kontroli dostępu.
- Brak blokowania pliku nad którym się pracuje (czasem zaleta bo lepiej rozwiązać konflikty niż blokować pracę).
- Problem z plikami dużych rozmiarów (pobieranie wszystkich z centralnego repozytorium).
- Nie jest zbyt przyjazny dla użytkownika (wysoki próg wejścia)

## Instalacja i konfiguracja

### Windows

- Instalacja za pomocą instalatora [Instalator](https://git-scm.com/download/win)
- Instalacja za pomocą `winget install git`

### Systemy linux oparte na dystrybucji debian

```bash
sudo apt update
sudo apt upgrade
sudo apt install git
```

## Konfiguracja

Kolejność pobierania konfiguracji:

1. Folder projektu
2. Folder użytkownika
3. Folder systemowy

```markdown
git config --local user.name
git config --global user.name
git config --system user.name
```

### Konfiguracja adresu email i nazwy użytkownika

```bash
# Ustawienie tożsamości
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# Ustawienie visual studio jako domyślny edytor
git config --global core.editor "code --wait"

# sprawdzenie bieżących ustawień
git config --list
# Wyświetlenie konfiguracji wraz z informacją o źródle jej pochodzenia
git config --list --show-origin
```

### Ustawienie vsc jako edytor podglądu zmian

```bash
# .gitconfig w katalogu użytkownika
git config --global -e

# lub
git config --edit --global
```

### Konfiguracja Visual Studio Code

> Konfiguracja VS Code jako domyślnego edytora:

```markdown
git config --global core.editor "code --wait"
```

> Konfiguracja VS Code jako Git diff tool:

```bash
# .gitconfig
[diff]
    tool = default-difftool
[difftool "default-difftool"]
    cmd = code --wait --diff $LOCAL $REMOTE
```

### Instalacja git-aliases dla powershell

```powershell
Install-Module git-aliases -Scope CurrentUser -AllowClobber
```

Jeśli podczas instalacji wystąpi błąd konieczna będzie zmiana uprawnień

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Konieczne jest dodanie importu w pliku profilu:

```powershell
code $PROFILE
```

```conf
Import-Module git-aliases -DisableNameChecking
```

## Przepływ pracy

1. Pobranie kodu z serwera kontroli wersji
2. Dodanie własnego kodu
3. Pobranie zmian z serwera jeśli ktoś pracował na tym samym pliku
4. Rozwiązanie konfliktów
5. Wysłanie zmian na serwer

### Stany w których mogą znajdować się pliki

- **Zatwierdzony** oznacza, że dane zostały bezpiecznie zachowane w Twojej lokalnej bazie danych.
- **Zmodyfikowany** oznacza, że plik został zmieniony, ale zmiany nie zostały wprowadzone do bazy danych.
- **Śledzony** - oznacza, że zmodyfikowany plik został przeznaczony do zatwierdzenia w bieżącej postaci w następnej operacji commit.

### Obszary robocze

- **Folder roboczy:** Lokalizacja na dysku w którym przechowywane są bieżące pliki i foldery, pliki są tworzone, modyfikowane lub kasowane.
- **Poczekalnia:** Miejsce do którego trafiają pliki przed zatwierdzeniem. Znajduje się pomiędzy obszarem roboczym a repozytorium. (Polecenie `add` dodaje pliki do poczekalni a `commit` przenosi je do repozytorium).
- **Repozytorium:** Całe repozytorium Git przechowywane jest w katalogu `.git`. Przechowuje on zatwierdzenia oraz całą historie projektu.

Katalog `objects` wewnątrz katalogu `.git` przechowuje różne typy obiektów:

- blob (zawartość plików w pewnym momencie historii)
- tree (reprezentują foldery w projekcie)
- commit (są tworzone za każdym razem kiedy wykonujemy zatwierdzenie commit)

:bulb: Wszystkie te obiekty są niezmienne, można je dodawać, usuwać ale nigdy nie można ich zmienić.

### Inicjalizacja nowego repozytorium GIT

Polecenie `git init` stworzy nowy podkatalog o nazwie `.git`, zawierający wszystkie niezbędne pliki.

:exclamation: Nie należy inicjalizować repozytorium w już istniejącym repozytorium.

```bash
# Inicjalizacja nowego reppozytorium
git init

# Inicjalizacja nowego repozytorium bez folderu roboczego
git init --bare
```

### Klonowanie istniejącego repozytorium GIT

```bash
git clone https://github.com/libgit2/libgit2

# Zapisanie repozytorium w katalogu o wybranej nazwie
git clone https://github.com/libgit2/libgit2 <my_folder_name>
```

### Sprawdzanie stanu plików

```bash
# Wersja pełna
## gst
git status

# Wersja skrócona
## gss
git status -s

# Szczegóły commita
git show <hash_commit>
```

### Śledzenie nowych plików

Po dodaniu nowego pliku do obszaru roboczego Git sygnalizuje że widzi nowy plik ale nie jest jeszcze przez niego śledzony (untracked).
Aby git mógł go śledzić konieczne jest dodanie go do indeksu za pomocą `git add <file_name`.

```bash
# Dodanie wszystkich plików
git add .

# Dodanie jednego pliku
git add <file_name>
```

Od teraz Git śledzi dodane pliki po edycji należy kolejny raz dodać zmiany dodać je do `staging area`

```bash
git add .
# lub
git add <file_name>
```

### Dodawanie zmodyfikowanych plików do poczekalni

```bash
# Dodanie zmian w plikach do poczekalni
git add .
git add -A

# Dodanie zmian w pojedynczym pliku do poczekalni
git add <file_name>

# Dodanie katalogu
git add folder/

# Podzielenie zmian na porcje które mają trafić do poczekalni
git add --patch <file_name>
```

:bulb: Wszystko co zostało dodane do poczekalni trafi do następnego zatwierdzenia.

### Ignorowanie plików

Jeśli chcemy wykluczyć pliki lub foldery z repozytorium git należy utworzyć plik `.gitignore` i dodać reguły co ma być ignorowane w folderze roboczym. Istnieje możliwość pobrania gotowych szablonów dla różnych języków i środowisk programistycznych [link](https://github.com/github/gitignore). W VS Code możemy pobrać rozszeżenie ktore wygeneruje ten plik automatycznie.

### Podgląd zmian w poczekalni i poza nią

Polecenie `git diff` służy do porównywania zmian pomiędzy trzema obszarami w których mogą znajdować się pliki (folderem roboczym, poczekalnią, repozytorium)

```bash
# Narzędzie diftool
git difftool

# Porównuje zmiany pomiędzy katalogiem roboczym a poczekalnią
git diff

# Porównuje zmiany pomiędzy plikami znajdującymi się w poczekalni i tymi które są w repozytorium
git diff --cached

# Porównuje zmiany od ostatniego zatwierdzeniem (zmiany które są aktualnie w poczekalni i katalogu roboczym)
git diff HEAD

# Porównuje zmiany pomiędzy ostatnim zatwierdzeniem a plikami znajdującymi się w poczekalni
git diff --staged

# Sprawdza zmiany pomiędzy gałęziami
git diff <branch1_name> <branch2_name>

# Sprawdza zmiany w konkretnym pliku pomiędzy gałęziami
git diff <branch1_name> <branch2_name> <file_name>

# Sprawdza różnicę pomiędzy dwoma zatwierdzeniami
git diff <commit> <commit>

# Ignoruje białe znaki
git diff -w

# Sprawdzenie różnicy pomiędzy aktualnym zatwierdzeniem a wcześniejszym (dwa zatwierdzenia)
git diff HEAD HEAD~2
```

### Zatwierdzanie zmian

```bash
# Uruchomienie przed zatwierdzeniem domyślnego edytora, który umożliwi wprowaszenie komentarza do zmian
git commit

# Zatwierdzenie zmian z poczekalni
git commit -m "Messaage"

# Automatyczne dodanie zmian do poczekalni podczas zatwierdzenia
git commit -am "Message"
```

:bulb: Każdy plik który został utworzony lub zmodyfikowany, a na którym później nie zostało uruchomione `git add` nie zostanie uwzględniony w zatwierdzeniu.

### Usuwanie pliku

Aby usunąć plik z Gita, należy go najpierw wyrzucić ze zbioru plików śledzonych, a następnie zatwierdzić zmiany.

Jeśli usuniemy plik systemowo git zobaczy że go nie ma i podczas sprawdzania poleceniem `git status` zaoferuje opcje przywrócenia, usunięcia lub dodania zmian do staging area.

```bash
# Usuwa plik z poczekalni ale zachowuje go w katalogu roboczym, po tym poleceniu plik przestaje być śledzony przez Git
git rm --cached

# Usuwa plik z poczekalni oraz katalogu roboczego
git rm -f

# Pokazuje które nieśledzone pliki zostaną usunięte z katalogu roboczego
git clean -dn

# usuwa plik nieśledzone z katalogu roboczego
git clean -df
```

### Zmiana nazwy pliku

Zmiana nazwy poprzez polecenie systemowe:

Po zamienie nazwy `git status` zobaczy starą nazwę jako plik usunięty a nową jako plik dodany, przy wykonaniu `git add` zmiani się status na `renamed`.

```bash
mv <old_file_name> <new_file_name>
```

Możemy również wykorzystać polecenie wbudowane w Git:

```bash
git mv <old_file_name> <new_file_name>
```

Będzie to równoważne w wykonaniem poleceń

```bash
mv File.txt File
git rm File.txt
git add File
```

### Podgląd historii rewizji

Domyślnie, polecenie git log uruchomione bez argumentów, listuje zmiany zatwierdzone w tym repozytorium w odwrotnej kolejności chronologicznej, czyli pokazując najnowsze zmiany w pierwszej kolejności.

```bash
# Wszystkie zmiany
## glgg
git log

# Liczba ostatnich zmian
git log -3

# Zmiany w jednej lini
## glog
git log --oneline

# Pokazuje log dla wybranej gałęzi
git log <branch_name> --online

# Zmiany w jednej linii z przedstawieniem graficznym gałęzi
git log --graph --decorate --oneline

# Zmiany dla commit na którym jest znacznik HEAD
git log HEAD

# Zmiany dla commit na którym jest rodzicem dla aktualnego HEAD
git log HEAD^

# Cofnij się o dwa commity od HEAD i go pokaż
git log HEAD~2
```

Zmiany Szczegółowe:

```bash
# zmiany szczegółowe
## glg
git log --stat

# zmiany wraz ze ze zmodyfikowanymi wartościami w plikach
## glgp
git log --stat -p
```


---------------
REFACTOR
---------------

Zwykle przepływ pracy odbywa się ze strony lewej do prawej
`working directory` -> `index` -> `repository`

Jeśli chcemy zmienić kierunek przepływu możemy wykorzystać polecenie `git checkout`.

Pozwala ono na:

* Zmianę położenia wskaźnika `HEAD` na inna gałąź (pośrednio będzie wskazywało na wybrany commit na tej gałęzi)
* Pobierze dane z tego zatwierdzenia i przywróci (skopiuje) z niego pliki do katalogu roboczego i indeksu (pliki we wszystkich obszarach są identyczne i zgodne z commitem na który wskazuje `HEAD`).


> Stash
Schowek w którym możemy umieszczać plik które nie są dodane jeszcze do indeksu

Zatwierdzenie to migawka obszaru roboczego w danym momencie. Różne zatwierdzenia mogą dzielić wspólne obiekty.
W historii każde zatwierdzenie wskazuje na swojego rodzica.

Gałęzie są referencjami do określonych zatwierdzeń (commit). Zatwierdzenie może należeć do wielu gałęzi.

Wskaźnik `HEAD` jest tylko jeden, zwykle wskazuje na bieżącą gałąź ze znajdującym się na niej zatwierdzeniem



Główna gałąź to `Main` (dawniej Master)

Repozytoria

* Lokalne (Local)
* Zdalne (Origin)

### Detached HEAD

Znacznik `HEAD` zwykle wskazuje na branch, ale Git może pracować również w trybie `odłączonej głowy`, oznacza to że wskaźnik `Head` nie zawsze musi wskazywać na gałąź. Może wskazywać na konkretny commit poprzez polecenie `git checkout <hash_commit>` (może to być aktualny commit).
Możemy wtedy dodawać kod i eksperymentować, jeśli nie zdecydujemy się na zapisanie zmian wskaźnik przełączamy na gałąź np. `main` poprzez `git switch main`
Niezapisane commity przestaną po jakimś czasie istnieć w bazie danych wyczyszczone przez **garbage collector**, możemy je jednak zapisać na nowej gałęzi korzystając z ich **hash** do póki są widoczne w logach konsoli.
W trybie odłączonej głowy nadal możemy tworzyć commity, należy wówczas przenieść się na wybrany commit `git checkout <hash_commit` i przypiąć go do jakiegoś brancha np. `git checkout <branch_name` lub wykonać `merge` do istniejącej gałęzi.

Miejsce wskaźnika możemy sprawdzić za pomocą `git branch`.

## REBASE

> :exclamation: Nie należy używać `rebase` na gałęziach publicznych

* Oczyszczanie historii gałęzi przed jej wysłaniem
* Pobranie zmian z jednej do drugiej gałęzi bez `merge`

### Zmiany historii lokalnej przed wysłaniem zmian do zdalnego repozytorium

## Dodanie kolejnych zmian w projekcie / pliku do wcześniejszego commit

```bash
git commit --amend
```

:bulb: Podczas commit z parametrem `amend` git nie modyfikuje ostatniego commit ponieważ są one niezmienne zamiast tego tworzy nową kopie z dodaną zawartością i przenosi nia nią znacznik `HEAD`, stary commit zostanie w przyszłości usunięty przez **garbage collector**. Działa tylko dla ostatniego zatwierdzenia.

:exclamation: Polecenie zmienia historie więc stosujemy tylko na gałęziach lokalnych.

Jeśli chcemy naprawić/zmienić historie wcześniejszych commitów musimy upewnić się że zmiany nie zostały wysłane jeszcze do repozytorium zdalnego.

<!-- Tworzymy nowy commit który w którym dodajemy wymagane zmiany a następnie wykorzystujemy polecenie rebase w trybie **interactive** -->

:bulb: Interaktywny **rebase** możemy wykorzystać do edycji/poprawy/scalenia historii przed wysłaniem do zdalnego repozytorium.

```bash
# wybieramy commit poniżej tego od którego chcemy zmienić historię
git rebase -i <hash_commit>
```

### MERGE i REBASE

> Narzędzie scalania

```bash
git mergetool
```

Scalanie `merge` polega na połączeniu dwóch gałęzi w jedną, czyli na. włączenie gałęzi z nowymi funkcjonalnościami do gałęzi `main`. Gdy jesteśmy na gałęzi main wykonujemy łączenie za pomocą komendy `git marge <feature_branch>`. Łączenie za pomocą samego `merge` tworzy nową migawkę która jest wspólna dla obydwu gałęzi.

Innym sposobem łączenia jest `rebase`.
 Polega to na zaciągnięciu zmian z poziomu `feature` z gałęzi `main` poprzez `git rebase master`, a następnie wykonania scalenia z poziomu gałęzi `main` poleceniem `git merge feature`. Przy tym sposobie commity zachowują swoją historię zmian. Przy stosowaniu tego sposobu ważne jest częste zaciąganie/rebase zmian z gałęzi master aby unikać dużej ilości konfliktów przy łączeniu.

:exclamation: Historia jest zachowana na jednej lini czasowej ale nie odzwierciedla dokładnie tego co się działo w projekcie

* `git switch main` Aktualizujemy main `git pull --rebase` (przy korzystaniu z repozytorium zdalnego)
* Zamieniamy branch `git switch feature` a następnie `git rebase main`
* Zmieniamy gałąź na main `git switch main` i scalamy `git merge feature`

### Cherry Pick

> `git cherry-pick` skopiować commita (bądź kilka commitów) z jednego brancha na drugi

```bash
git cherry-pick <hash_commit>

# po naprawieniu konfliktu
git cherry-pick --continue
```

### TAGI

Tagi to rodzaj zakładki przy pomocy których możemy oznaczać ważne commity. Najczęściej oznaczmy commity które wskazują koniec pracy nad daną funkcjonalnością lub kolejną wersją aplikacji.

Tagi dzielimy na dwie kategorie

* lekkie (bez adnotacji)
* z adnotacją (tworzą w strukturze dodatkowe miejsce na zapisanie dodatkowych danych)

Flaga `a` dodaje dodatkowe adnotacje np. o autorze

```bash
# Dodanie tag
# a - annotation
# m - message
git tag <tag_name> -a -m <message>

# Dodanie tag do dowolnego commit można dodać również -a -m
## git tag v1.0 3432340
git tag <tag_name> <hash_commit>

# Wyświetlenie tagów
git tag

# Tag bez adnotacji
git tag <tag_name>


# dodatkowe informacje o commit na który wskazuje TAG
git show <tag_name>

# Usunięcie Tagu
git tag -d <tag_name>

# Wysłanie samych tagów do zdalnego repozytorium
git push --tags

# Usuwanie tagu ze zdalnego repozytorium
## git push -d origin v1.0
git push -d <remote_repository_name> <tag_name>
```

## Polecenia systemowe

```bash
# aktualny katalog roboczy
pwd
```

## Praca z GIT Command Line

> Zatwierdzenie zmian z komentarzem

```bash
git commit -m "Init Commit"
```

> Dodanie lokalnego repozytorium do repozytorium zdalnego GitHub

```bash
git remote add origin https://github.com/{user}/{repo-name}.git

# Zmiana nazwy gałęzi
git branch -M main
git push -u origin main
```

## Podstawowe komendy



> Wyświetla pliki w katalogu
W zależności od przełącznika wyświetla wybrane pliki w katalogu
[git ls-files](https://git-scm.com/docs/git-ls-files)

```bash
# Wszystkie śledzone pliki
git ls-files

```

## Odzyskiwanie utraconych zmian

Reflog pozwala na dokładne przeglądanie historii (po których elementach poruszał się znacznik `HEAD`). możemy z niego odzyskać dane np. hash zmian które zostały wprowadzone poprzez wykorzystanie interaktywnego rebase. Dane są widoczne do momentu zebrania ich przez **garbage collector**.
Reflog zawiera dane lokalne.

możemy również odzyskać dane poprzez

```bash
git reflog
git checkout <hash>
git switch -c <new_branch_name>
git checkout main
git merge <branch>
git branch -d <branch>
```

```bash
git reflog
```

> Sprawdzenie jaka które commit zmieniały wartość zawartość pliku

```bash
git blame <file_name>
```

Pliki mogą być zmodyfikowane w różnych miejscach

| Staged | Modified | File Name |
| ------ | -------- | --------- |
| M      |          | File1     |
| M      | M        | File2     |
| A      |          | File3     |
| ?      | ?        | File4     |

Oznaczenia liter

* Nowe nieśledzone pliki mają obok siebie `??`
* Nowe pliki dodane do poczekalni `A`
* Zmodyfikowane pliki `M`

Lewa strona wskazuje na to czy plik jest w poczekalni, a prawa czy jest zmieniony.


## Wycofanie wprowadzonych zmian

Przy zastosowaniu polecenia `git checkout <hash_commit>` przechodzimy w tryb `detached HEAD` (tryb odłączonej głowy 😄). Pozwala to przeglądać i eksperymentować bez zapisywania zmian. Jeśli jednak zdecydujemy się na ich zapisanie możemy użyć komendy `git checkout -b <branch_name>`.

### Zmiany które nie zostały dodane do staged

Istnieją różne sposoby przywracania projektu jak również pojedynczego pliku.

> `git checkout` - służy do przywracania projektu lub pliku do wcześniejszego stanu.

```bash
# przywraca wybrany plik jeśli nie był dodany do staging (tylko zmodyfikowany)
git checkout <file_name>
# przywraca wszystkie pliki jeśli nie były dodane do staging (tylko zmodyfikowany)
git checkout .
```

`checkout` przesuwa wskaźnik `HEAD` na wskazany commit. W katalogu zostaną wczytane pliki z określonego commitu.

Możemy użyć również `git restore`

```bash
# przywraca wybrany plik jeśli nie był dodany do staging (tylko zmodyfikowany)
git restore <file_name>
# przywraca wszystkie pliki jeśli nie były dodane do staging (tylko zmodyfikowany)
git restore .
```

### Zmiany które zostały dodane do staged

Gdy dodamy pliki do `stage` za pomocą `git add <file_name>` możemy wycofać ten stage za pomocą `git restore --stage` a następnie tak jak w przypadku pliku który był tylko zmodyfikowany przywrócić jego stan z ostatniej migawki za pomocą `git restore <file_name>`

```bash
git restore --stage <file_name>
git restore <file_name>
```

Możemy również użyć `git reset` w sytuacji gdy wprowadziliśmy już zmiany do indeksu za pomocą `git add` i chcemy je wycofać.
Jesteśmy przenoszeni do zatwierdzenia na które wskazuje znacznik `HEAD` a on wskazuje na ostatni wykonany commit w tej sytuacji gałąź tak naprawdę nie jest przesuwana ponieważ wskaźnik cały czas się tam znajduje.

Możemy również przywrócić pojedynczy plik z ostatniego commit `git reset <file_name`.

``` bash
# Przywrócenie wybranego commit
git checkout <hash_commit>

# zapisanie zmian na wybranej gałęzi
git checkout -b <branch_name>

# powrót na wybraną gałąź
git checkout master

# nowsza wersja
git switch master
```

### Zmiany które zostały dodane do repozytorium

Zmiany które zostały zatwierdzone przez `commit`

> `git reset`

Polecenie zmienia bieżącą gałąź a co się z tym wiąże zmienia również bieżące zatwierdzenie.
`git reset` to wyspecjalizowane polecenie dzięki któremu możemy przenieść się na dowolny commit (poprzez zmianę gałęzi). Zmiana jest dokonywana razem ze znacznikiem `HEAD`. Aktualnym zatwierdzeniem jest to wybrane przez `git reset`.

:exclamation: Powoduje modyfikację historii commit (usuwa późniejsze commit). Zmiany należy wycofać przed wysłaniem zmian do zdalnego repozytorium.

Jednym ze sposobów użycia `git reset` jest powrócenie do wcześniejszej wersji projektu, cofnięcie gałęzi w czasie bez zachowania późniejszej historii zatwierdzeń.
Jeśli chcemy przywrócić pliki do dokładnie takiej wersji jak w migawce we wszystkich obszarach skorzystamy z opcji `--hard`.
Ponieważ nie ma już odniesień do późniejszych zatwierdzeń (commit) zostaną one wyczyszczone po jakimś czasie przez `garbage collector`.

```bash
# Przesunie tylko gałąź i nie zmodyfikuje pozostałych obszarów
git reset --soft

#  skopiuje zmiany z repository do index (domyślnie jeśli nie dodamy żadnego parametru)
git reset --mixed

# skopiuje zmiany z repozytorium do wszystkich pozostałych obszarów (working area, index)
git reset --hard
```

Jeżeli nie zastosujemy żadnej opcji domyślnie będzie zastosowany `--mixed` i zmiany z repozytorium (z bieżącego commit) znajdą się tylko w obszarze index (plik będzie miał status `modified`).

```bash
git reset HEAD

# Przywrócenie wszystkich plików z repository do working directory
git reset HEAD --hard

```

Jeśli mamy więcej niż jeden plik i chcemy zastosować przywrócenie tylko na mim możemy przenieść zmiany z **repozytorium** do **stage** i nie zmieniać plików w **working directory** a jedynie plik który jest w **staging**.

```bash
# Przywrócenie tylko jednego pliku z repository do staging
git reset HEAD <file_name>
```

:exclamation: Niestety nie jest możliwe przywrócenie pojedynczego pliku z opcją `--hard` za miast tego należy użyć (w tym przypadku `checkout` nie spowoduje zmiany `HEAD`)

```bash
git checkout HEAD --hard <file_name>
```

> `git revert` - odwraca zmiany z wybranego commit i zapisuje jako nowy commit

Pozwala bezpiecznie przywracać zmiany które znajdują się już w repozytorium **zdalnym**.

:bulb: Nie usuwa nie modyfikuje historii zmian i odwraca zmiany tylko w wybranym commit (wcześniejsze commit pozostają bez zmian).

```bash
git revert <hash_commit>
```

> Pobieranie zmian ze zdalnego repozytorium

`git pull` to połączenie `git fetch` i `git rebase`

```bash
git pull --rebase

# pobranie wszystkich gałęzi
git pull --all
```

> Dodanie nowej gałęzi/branch

```bash
git checkout -b <branch_name>

# nowsza wersja
git switch -c <branch_name>
```

> Zmiana nazwy gałęzi

```bash
# Zmiana nazwy gałęzi na której się obecnie znajdujemy
git branch -m <new_branch_name>

# zmiana nazwy gałęzi na której się nie znajdujemy
git branch -m <old_branch_name> <new_branch_name>
```

> Przełączenie się pomiędzy gałęziami/branch

```bash
# git checkout master
git checkout <branch_name>

# nowsza wersja
git switch <branch_name>
```

> Sprawdzenie dostępnych gałęzi/branch

```bash
git branch
```

> Odkładanie zmian na stos / do schowka (stash)

Jeśli wprowadzamy zmiany na gałęzi w śledzonym pliku i nie są one zatwierdzone przy pomocy commit przy próbie zmiany gałęzi Git nie pozwoli się na nią przełączyć bez ich zatwierdzenia. Jeśli nie chcemy tego robić możemy zmiany zapisać w schowku.

Zmiany zostaną wycofane (do momentu wcześniejszego commit) i zapisane w schowku.

Przy przenoszeniu danych do schowka są tam zapisywane a do **working directory** są kopiowane pliki z ostatniego zatwierdzenia (commit).

W schowku przechowujemy rzeczy które musimy odłożyć na pewien czas ale nie chcemy ich dodawać do **staging area**.
W schowku są scalone w jeden identyfikator i jest to schowek wielokrotny.

```bash
git stash

# umieszczenie w schowku z dodanym komunikatem
git stash push -m "message"

# dodaje nieśledzone pliki do schowka (domyślnie git ignoruje pliki które nie są śledzone)
git stash --include-untracked
```

> Podejrzenie odłożonych zmian na stosie wraz z identyfikatorem

```bash
git stash list
# stash@{0}: On feature: Nowa zmiana w schowku


> Aby je przywrócić lub dodać do innej gałęzi wykorzystujemy polecenie
Jeśli nie podamy identyfikatora polecenia przywrócą ostatnią zmianę

```bash
# przywraca zmiany i usuwa je ze stosu
git stash pop

# przywraca zmiany i pozostawia na stosie
git stash apply

# przywrócenie konkretnego identyfikatora
git stash pop stash@{0}
```

## więcej szczegółów

```bash
git stash show
```

> Usunięcie zmiany ze schowka

```bash
# wyczyszczenie konkretnej zmiany
git stash drop stash@{0}

# wyczyszczenie całego schowka
git stash clear
```

> Utworzenie nowego branch ze zmian zapisanych na stosie

```bash
git stash branch <new_branch_name>
```

> Informacje o zdalnym repozytorium

```bash
git remote
git remote -v

```

> Dodanie zdalnego repozytorium

```bash
git remote add <name> <remote_url>
```

> Wypychanie zmian do zdalnego repozytorium

```bash
# git push origin master
## gp
# -f force, -u default upstream
git push <remote_name> <branch_name>
```

> Usunięcie gałęzi lokalne

```bash
# Usunięcie gałęzi lokalnie
git branch -d <branch_name>

# usunięcie gałęzi która ma zatwierdzenia i nie została scalona z inną gałęzią
git branch -D <branch_name>

# Usunięcie gałęzi na zdalnym repozytorium
# git push -d origin <branch_name>
git push -d <remote_name> <branch_name>
```



> Scalanie zmian z innej gałęzi/ **Merge**

Jeśli podczas rozwoju gałęzi np. feature główna gałąź nie została zmieniona i nie ma konfliktów zastosowany zostanie `fast forward`.

```bash
# Przełączamy się na branch do którego chcemy dodać zmiany przeważnie main
git checkout main
git merge <branch_name>
```

## Rozwiązywanie konfliktów

W pliku oznaczenie `<<<HEAD` to zatwierdzenie bieżące

> Po rozwiązaniu konfliktu

```bash
# po edycji i rozwiązaniu pliku musimy go dodać do stage
git add <file_name>

# następnie wykonujemy commit
git commit -m "commit_message"

git rebase --continue
```


## Narzędzia

## Praca z GIT

### Podstawkowe zasady

* Podczas wprowadzania nowej funkcjonalności tworzymy nowy `branch`

## Do Sprawdzenia

Przy korzystaniu z WSL i personal token na github konieczne podlinkowanie credential managera z windowsa do debiana

```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```

C:\Users\kudzi\OneDrive\Dokumenty\WindowsPowerShell\Modules\oh-my-posh\3.175.0\themes
