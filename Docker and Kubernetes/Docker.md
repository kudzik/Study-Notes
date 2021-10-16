# Docker

## Linki

[play-with-docker](https://labs.play-with-docker.com/)

[Docker posh powershell](https://www.powershellgallery.com/packages/posh-docker/)

## Przepływ pracy

Z kodu źródłowego aplikacji przy pomocy Dockera tworzymy obraz kontenera wypychamy obraz do rejestru który możemy następnie uruchomić jako kontener.

Katalog aplikacji zawiera plik o nazwie `Dockerfile` na podstawie którego `docker image build` tworzy obraz kontenera. Ściąga potrzebne zależności, kopiuje pliki i wszystko zostaje spakowane w jeden obraz.

Obraz przesyłamy do rejestru za pomocą polecenia `docker image push`.

Uruchomienie obrazu odbywa się za pomocą `docker container run`

## Informacje o programie

### Informacje o zainstalowanej wersji

```bash
# Informacje o wersji
docker --version
```
## Obrazy

Obraz jest przepisem na zbudowanie kontenera i zawiera wszystkie konieczne do uruchomienia warstwy. Jest tylko do odczytu, budujemy go zawierając w nim naszą aplikację. Obrazu nie można zaktualizować, jeśli dokonamy zmian to wydajemy nową wersję.

### Wyświetlanie informacji o obrazach

```bash
# Informacje o istniejących obrazach
docker images
```
### Budowanie obrazu z folderu aplikacji

```bash
docker image build -t kudzik/webapp:first .
docker build -t <registry>/<name>:<tag> .
```

`kudzik` - Docker Hub ID
`/webapp` - Nazwa repozytorium
`:first` - nazwa obrazu
`.` - kontekst oznacza ścieżkę względem miejsca z którego wykonujemy polecenie 

```bash
# -t nadanie tagu
# -f podanie nazwy pliku innego niż domyślny Dockerfile

```

:bulb: Jeśli wyjdziemy z trybu interaktywnego przy pomocy `exit` lub `CTRL+C` kontener zostanie zabity, aby wyjść i pozostawić go uruchomionego korzystamy ze skrótu `CTRL+P+Q`.

### Przesłanie zbudowanego obrazu do repozytorium

Jeśli chcemy przesyłać obrazy repozytorium musimy oznaczyć je prawidłowo tagiem, przykładowo dla docker hub będzie to nasza nazwa użytkownika `<userName>/<RepoName>:<version>`, `<userName>/<ImageName>:<version>`

Przed przesłaniem obrazu konieczne jest zalogowanie się do docker hub.

```markdown
docker login
```

Nazwa repozytorium jest tożsama z nazwą obrazu, nie możemy ściągnąć aplikacji bez podania jej wersji, jeśli jej nie podamy domyślnie zostanie ściągnięta wersja najnowsza (latest).

```markdown
docker image push  kudzik/webapp
docker push  kudzik/webapp
```

### Usuwanie obrazu


```bash
docker image rm <containerId>
docker rmi <containerId>
```
## Docker file

[Docker file Docs](https://docs.docker.com/engine/reference/builder/)
Docker file jest plikiem który zawiera polecenia dzięki którym możliwe jest zbudowanie obrazu aplikacji, każda instrukcja będzie osobną warstwą w obrazie.

```dockerfile
# Wskazanie wersji obrazu na podstawie którego ma być zbudowany nasz obraz, zawiera zainstalowane narzędzia jak np. środowisko startowe.
FROM mcr.microsoft.com/dotnet/aspnet:6.0-focal AS base

# Metadane, autor, data utworzenia,
LABEL author="Batman"

# Definicje zmiennych środowiskowych
ENV NODE_ENV=production
ENV PORT=3000

## WORKDIR ustawia kontekst folderu naszej aplikacji
WORKDIR /app

# RUN pozwala na wykonywanie komend wiersza poleceń systemu w którym jest uruchamiana aplikacja
RUN dotnet restore "HelloApi.csproj"

# COPY kopiuje pliki z folderu naszej aplikacji do folderów znajdujących się w obrazie
COPY ["HelloApi.csproj", "./"]
# . - oznacza wszystkie pliki
COPY . .
# Wszystkie pliki do określonego katalogu
COPY . /usr/src/app

# Wykonuje polecenia
RUN npm install

# Port na którym kontener będzie nasłuchiwał
EXPOSE 3000
# Dostęp do zmiennej zdefiniowanej wcześniej w ENV 
EXPOSE $PORT

# ENTRYPOINT Polecenie wykonuje się zawsze podczas uruchamiania kontenera
ENTRYPOINT ["dotnet", "HelloApi.dll"]

```

## Docker Ignore

W pliku `.dockerignore` definiujemy które pliki i katalogi docker ma pominąć podczas budowania aplikacji, kopiowania plików. Np. `node-modules` dla node.



## Kontenery



Kontener jest budowany na podstawie obrazu dokera, możemy go uruchomić, zatrzymać, przenosić i usuwać, możemy powołać dożycia wiele kontenerów stworzonych z tego samego obrazu.

Kontener to w zasadzie kolejna warstwa dodawana do obrazu z tą różnicą że jest do odczytu i zapisu. 

### Sprawdzenie uruchomionych kontenerów

```bash
# Wyświetlenie działających kontenerów
docker container ls
docker ps

# Wszystkie kontenery
docker ps -a

## -a wszystkie
```

:bulb: Jeśli chcemy sprawdzić wszystkie kontenery nawet te które są zatrzymane musimy dodać flagę `-a`.

### Utworzenie kontenera z obrazu

```bash
docker run -p <externalPort>:<internalPort> <imageName>
docker run --name <containerName> -p 8080:8080 kudzik/webapp
# Kontener uruchomi się w tle
docker run -d --name <containerName> -p 8080:8080 kudzik/webapp

## -p port
## -d detach
```

:bulb: Jeśli podczas tworzenia kontenera chcemy odłączyć się od jego terminala (uruchomi się w tle) musimy zastosować flagę `-d`

### Uruchomienie istniejącego kontenera

```bash
docker container start <containerId>
```

### Zatrzymanie kontenera

```bash
docker container stop <containerId>
```

### Usuwanie kontenera

Kontener przed usunięciem musi zostać zatrzymany, jeśli chcemy wymusić usunięcie działającego kontenera stosujemy flagę `-f`.

```bash
docker container rm <containerId>
docker container rm -f <containerId>
```

### Podłączenie się w trybie interaktywnym do zbudowanego kontenera

```bash
# Wejście do powłoki podczas tworzenia kontenera
docker container run -it --name linux alpine sh

# Podłączenie się do działającego kontenera
docker exec -it <containerId> sh

```
:bulb: `sh` to powłoka ktora może sie różnić w zależności od używanego obrazu.

### Logi kontenera

```bash
docker logs <container>
```

## Docker Volumes

Woluminy służą do zapisu informacji poza kontenerem, dzięki temu po usunięciu kontenera nie stracimy informacji. Pliki będą nadal przechowywane w wydzielonym przez nas miejscu na dysku. Logi domyślnie są składowane na hoście dokera.

Wolumen możemy podłączyć do obrazu dzięki temu gdy zmienimy plik w folderze projektu zmiany będą widoczne w kontenerze

:bulb: Taki wolumen możemy użyć do przechowywania np. logów dzięki temu po awarii kontenera nie stracimy informacji o tym co się wydarzyło.

```bash

# zabierz wszystkie z katalogu /var/www/logs i prześlij do hosta dokera
docker run --name some-nginx-alpine-logs -d -p 8080:80 -v /var/www/logs nginx:alpine

# linux/mac w aktualnym katalog
docker run --name some-nginx-alpine-logs-pwd -d -p 8080:80 -v $(pwd)/logs:/var/www/logs nginx:alpine

# Windows
docker run --name some-nginx-alpine-logs-pwd -d -p 8080:80 -v ${PWD}/logs:/var/www/logs nginx:alpine
```

## Komunikacja pomiędzy kontenerami

[Networking overview](https://docs.docker.com/network/)

Kontenery komunikują się pomiędzy sobą za pomocą sieci. Najczęściej jest to most (bridge network) działający w obrębie jednej maszyny. 

### Informacje o sieci

```bash
docker network ls

# szczegółowe informacje o sieci łącznie z podpiętymi do niej kontenerami
docker network inspect <containerId>
```
### Tworzenie sieci

```bash
docker network create --driver <networkType> <networkName>
docker network create --driver bridge my_network
```

### Usuwanie istniejącej sieci

```bash
docker network rm <networkName>
```

### Dodawanie kontenera do sieci

```bash
docker run -d --net=<network_name> --name=<container_name> <image_name>
docker run -d --network=<network_name> --name=<container_name> <image_name>
```

:bulb: Ważne aby podczas tworzenia kontenera nadać mu nazwę której później będziemy używać w kodzie aplikacji aby się do niego odwołać np. w connection stringach. 

