# Docker and Kubernetes

## Linki

[play-with-docker](https://labs.play-with-docker.com/)
[Docker posh powershell](https://www.powershellgallery.com/packages/posh-docker/)

## Przepływ pracy

Z kodu źródłowego aplikacji przy pomocy Dockera tworzymy obraz kontenera wypychamy obraz do rejestru który możemy następnie uruchomić jako kontener.

Katalog aplikacji zawiera plik o nazwie `Dockerfile` na podstawie którego `docker image build` tworzy obraz kontenera. Ściąga potrzebne zależności, kopiuje pliki i wszystko zostaje spakowane w jeden obraz.

Obraz przesyłamy do rejestru za pomocą polecenia `docker image push`.

Uruchomienie obrazu odbywa się za pomocą `docker container run`

## Informacje

> Informacje o zainstalowanej wersji

```bash
# Informacje o wersji
docker --version
```
> Logowanie do Docker Hub

```markdown
docker login
```

## Obrazy

### Docker file

```dockerfile
# Wskazanie wersji obrazu na podstawie którego ma być zbudowany nasz obraz, zawiera zainstalowane narzędzia jak np. środowisko startowe.
FROM mcr.microsoft.com/dotnet/aspnet:6.0-focal AS base

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

# ENTRYPOINT Polecenie wykonuje się zawsze podczas uruchamiania kontenera
ENTRYPOINT ["dotnet", "HelloApi.dll"]

```

### Budowanie obrazu z folderu aplikacji

```bash
# dodanie tagu -t
docker image build -t kudzik/webapitest:first .
```

`kudzik` - Docker Hub ID
`/webapitest` - Nazwa repozytorium
`:first` - nazwa obrazu
`.` - kontekst (wszystkie pliki w folderze)

### Podłączenie się w trybie interaktywnym do zbudowanego kontenera

```bash
# obraz alpine
# powłoka sh
docker container run -it --name linux alpine sh

```

:bulb: Jeśli wyjdziemy z trybu interaktywnego przy pomocy `exit` lub `CTRL+C` kontener zostanie zabity, aby wyjść i pozostawić go uruchomionego korzystamy ze skrótu `CTRL+P+Q`.

### Przesłanie zbudowanego obrazu do repozytorium

```markdown
docker image push  kudzik/webapitest
```

## Kontenery

> Sprawdzenie uruchomionych kontenerów

```bash
docker container ls
# lub
docker ps
```

:bulb: Jeśli chcemy sprawdzić wszystkie kontenery nawet te które są zatrzymane musimy dodać flagę `-a`.

### Utworzenie kontenera z obrazu

```bash
docker run --name <containerName> -p 8080:8080 kudzik/webapitest
# Kontener uruchomi się w tle
docker run -d --name <containerName> -p 8080:8080 kudzik/webapitest
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

> Informacje

```bash
docker ps -a
```

### Logi

```bash
docker logs <container>
```
