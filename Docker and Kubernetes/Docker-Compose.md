# Docker Compose

Docker Compose korzysta z plików konfiguracyjnych w formacie YAML. Można za pomocą niego zbudować jeden lub wiele kontenerów, uruchamiać i zatrzymywać kontenery a także wyświetlać status uruchomionych usług.

Obrazy są budowane i uruchamiane w sposób deklaratywny, zamiast poleceń wydawanych pojedynczo w konsoli.

## Zarządzanie obrazami

### Budowanie obrazów

Obrazy są budowane na podstawie pliku `Docker-compose.yml`

```bash
docker-compose build
```

### Uruchamianie obrazów

```bash
docker-compose up

# Uruchamianie razem z budowaniem
docker-compose up --build
```

### Zatrzymywanie kontenerów

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

```bash
# LINUX

export APP_ENV=<value>
export APP_ENV=development
export APP_ENV=production
```

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
:bulb: Można użyć nazwy środowiskowej aby do nazwy obrazu dodawany był nasz użytkownik np. `image: ${USERNAME_REGISTRY}/nodeapp`

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