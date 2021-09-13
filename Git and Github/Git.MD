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

