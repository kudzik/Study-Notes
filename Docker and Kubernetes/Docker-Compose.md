# Docker Compose

Docker Compose korzysta z plików konfiguracyjnych w formacie YAML. Można za pomocą niego zbudować jeden lub wiele kontenerów, uruchamiać i zatrzymywać kontenery a także wyświetlać status uruchomionych usług.

Obrazy są budowane i uruchamiane w sposób deklaratywny, zamiast poleceń wydawanych pojedynczo w konsoli.

## Zarządzanie obrazami

### Budowanie obrazów

Obrazy są budowane na podstawie pliku `Docker-compose.yml`

```bash
docker-compose build
```

### Tworzenie kontenerów

```bash
docker-compose up
docker-compose up -d

# Uruchamianie razem z budowaniem
docker-compose up --build

# Tworzenie kontenera bez przebudowywania wybranego
docker-compose up --np-deps node

# -d detach

```

### Skalowanie kontenerów

Możemy uruchomić więcej niż jedną instancję kontenera. Przy uruchomieniu wielu instancji nie możemy definiować portu zewnętrznego (zdefiniowany będzie zajęty przez pierwszy który się uruchomi). Definiujemy tylko port wewnętrzny. Nie definiujemy również jego nazwy. Możemy określić w pliku również co ma się wydarzyć podczas awarii, ile ma nastąpić prób ponownego uruchomienia a także czas oczekiwania pomiędzy ponownym uruchomieniem. 

```bash
docker-compose up -d --scale api=4
```

```yaml
services:
  redis:
    image: redis:latest
    deploy:
    # ilość instancji
      replicas: 2
    ports:
      - "3000"
```

### Usuwanie kontenerów

```bash
docker-compose down
```

### Przekazywanie zmiennej za pomocą ARGS

Możemy przekazać wartość zmiennej z pliku `docker-compose.yml` do pliku `Dockerfile`

```yml
# docker-compose.yml
version: '3.7'

services:

  node:
    container_name: nodeapp
    image: nodeapp
    build:
      context: .
      dockerfile: node.dockerfile
      args:
        buildversion: 1
```


```dockerfile
# Dockerfile

ARG buildversion
ENV build=$buildversion
RUN echo "Build version: $build"
```

### Używanie i zmienianie zmiennych środowiskowych

Jeśli użyjemy zmiennej w pliku, a nie będzie ustawiona otrzymamy ostrzeżenie podczas wykonania `docker compose ps`.

Sposoby definiowania zmiennych środowiskowych

- W pliku `docker-compose.yml` lub `Dockerfile`
- systemowe
- w pliku .env
- w edytorze np. Visual Studio. Dla ASP.NET `launchSettings.json`

W przypadku definicji zmiennej w pliku `docker-compose.yml` zostanie ona przekazana do kontenera (bezpośrednio lub przez plik `Dockerfile`) i kontener otrzyma do niej dostęp. Zmienna nie musi być podana na sztywno, może być zdefiniowana jako zmienna środowiskowa wtedy w pliku odwołamy się do niej przez jej nazwę.

```yaml
  node:
    container_name: nodeapp
    image: nodeapp
    environment:
      - NODE_ENV=production
      - APP_VERSION=1.0
```

Może być też umieszczona w pliku z rozszerzeniem `.env`.

```bash
# app.env
MONGODB_ROOT_USERNAME=user
MONGODB_ROOT_PASSWORD=password
MONGODB_ROOT_ROLE=root
```

```yaml
    node:
      container_name: mongo
      image: mongo
      env_file:
        - ./.docker/env/app.env
```

:bulb: Można użyć nazwy środowiskowej aby do nazwy obrazu dodawany był nasz użytkownik np. `image: ${USERNAME_REGISTRY}/nodeapp`

#### LINUX

```bash
export APP_ENV="<value>"
export APP_ENV="development"
export APP_ENV="production"

# Lista zmiennych zadeklarowanych
printenv
printenv | less
printenv | more

# Dostęp do wartości 
echo ${APP_ENV}

# Usunięcie zmiennej
unset APP_ENV
```
#### Windows

[Zmienne Windows](https://docs.microsoft.com/pl-pl/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.1)

```PowerShell
# PowerShell

$Env:<variable-name> = "<new-value>"
$Env:APP_ENV="development" 
$Env:APP_ENV="production"

# Dostęp do ustawionej wartości otrzymujemy poprzez podanie jej nazwy
$Env:APP_ENV

# Lista zmiennych zadeklarowanych
Get-ChildItem env:
ls env:

# Usunąć możemy poprzez przypisanie pustej wartości
$Env:APP_ENV=""
```

Dostęp do zmiennej z poziomu pliku `docker-compose.yml`

```yml
image: ${APP_ENV}
```

### Wypychanie obrazów do rejestru

```bash
docker-compose push
# Tylko wybrane
docker-compose push [service...]

```
### Woluminy

Woluminy [Docker Volumes](./Docker.md/#docker-volumes)

konfiguracja woluminów w pliku `docker-compose.yml`

```yaml
node:
      container_name: node_img
      image: my_node
      build: 
        context: .
        dockerfile: .docker/Dockerfile
      volumes:
        - ./logs:/var/www/logs
```

### Sieć


### Logi kontenerów

```bash
docker-compose logs
docker-compose logs [services...]
docker-compose logs tail=5
docker-compose logs --follow 
```

### Podłączanie się do kontenera

```bash
docker-compose exec <serviceName> <shell>
```

## Przykładowy plik docker-compose.yml

```yml
# docker-compose.yml

version: '3.7'

services:

  node:
    container_name: nodeapp
    # Nazwa jaką będzie maił obraz
    image: nodeapp
    build:
      context: .
      dockerfile: node.dockerfile
      args:
        PACKAGES: "nano wget curl"
    ports:
      - "3000:3000"
    networks:
      - nodeapp-network
    volumes:
      - ./logs:/var/www/logs
    environment:
      - NODE_ENV=production
      - APP_VERSION=1.0
    depends_on: 
      - mongodb
      
  web.service:
    build:
      context: .
      dockerfile: WebBrowser.Service/Dockerfile

  db:
    container_name: db
    image: "mcr.microsoft.com/mssql/server"
    environment:
      SA_PASSWORD: "Your_password123"
      ACCEPT_EULA: "Y"

  mongodb:
    container_name: mongodb
    image: mongo
    networks:
      - nodeapp-network

networks:
  nodeapp-network:
    driver: bridge
```