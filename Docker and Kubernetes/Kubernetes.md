# Kubernetes

## Linki

[API Docs](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.22/)

Został stworzony przez Google w 2015 roku. Jest następcą projektów Borg i Omega. Skrócona nazwa to K8S.

Kubernetes zarządza kontenerami na wysokim poziomie. Deklarujemy manifest czego oczekujemy, a Kubernetes zarządza nimi automatycznie, kontenery mogą budować mikroserwisy, które składają się z izolowanych usług. Architektura ta zastępuje aplikacje monolityczne, gdzie poszczególne moduły wchodziły w skład jednej dużej aplikacji.

Dzięki umieszczeniu każdej usługi w oddzielnym kontenerze możliwe jest skalowanie aplikacji poprzez zwiększenie liczby instancji danego serwisu, aktualizacja poszczególnych serwisów bez ponownej kompilacji całej aplikacji, a nawet rozbudowywanie i usuwanie błędów bez przerw w jej działaniu. komunikacja pomiędzy poszczególnymi serwisami zwykle realizowana jest poprzez API.

Kubernetes jest koordynatorem mikrousług.

Kubernetes można porównać do **trenera** drużyny piłkarskiej, dba o rozstawienie zawodników zgodnie z ich umiejętnościami, pilnuje by wszyscy trzymali się planu a także wymienia zawodników na innych gdy przydarzy się kontuzja. Jest również w stanie podejmować decyzje na bieżąca w zależności od zaistniałej sytuacji.

:bulb: W kubernetes serwisy możemy kreować na dwa sposoby imperatywny oraz **deklaratywny**, ten drugi jest sposobem preferowanym.

Tryb **imperatywny** to tryb w którym możemy konfigurować kubernetes poprzez `kubectl` wydając odpowiednie polecenia, **deklaratywny** opiera się na przygotowaniu plików `yml` z konfiguracją.


## Masters

**Klastry**
Przeważnie stosuje się 3 węzły master, powinny być na różnych maszynach. Liczba powinna być zawsze nieparzysta, chroni to przed problemami takimi jak **Split Brain** i **Deadlock**.
Tylko jeden węzeł master może wprowadzać zmiany i jest on nazywany **liderem**, pozostałe to węzły **Follower**. Jeśli lider ulegnie awarii wybierany jest nowy spośród działających followers.

Na hostingach od dostawców nie mamy dostępu do Masterów, są ukryte i zarządzane przez dostawcę, otrzymujemy tylko punkty końcowe API endpoint.

## Node (węzły robocze)

## Serwisy

> Dodanie serwisu typu NodePort w sposób imperatywny

Tworzy serwis z otwartą komunikacją do poda pracującego na porcie 8080, port dostępowy z zewnątrz znajduje się w informacjach o serwisie.

```bash
kubectl expose pod hello-pod --name=hello-svc --target-port=8080 --type=NodePort
```

> Usunięcie serwisu

```bash
kubectl get services # services możemy zastąpić aliasem svc
kubectl delete svc hello-svc
```

> Dodanie serwisu s sposób deklaratywny

```yml
apiVersion: v1
kind: Service
metadata:
  name: ps-nodeport
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 31111
    protocol: TCP
  selector:
    app: web
```

[IP serwisu](../Docker%20and%20Kubernetes/img/Kubernetes_service_IP.jpg)

## Pods

Pod (kapsułka) jest główną jednostką skalowania. Skalujemy zawsze dodając nowe kapsułki, nie dodajemy nowych kontenerów do kapsułek.

Kontenery w Kubernetes zawsze są umieszczane w Pods, nie można wdrożyć kontenera bezpośrednio. Można uruchomić wiele kontenerów w jednym Podzie.  Każda kapsułka otrzymuje środowisko wykonawcze, a działające kontenery to środowisko współdzielą, będą posiadać ten sam adres IP i współdzielić zasoby (pamięć, volumes). Aby dostać się do konkretnej usługi z zewnątrz trzeba odwołać się do adresu IP i konkretnego portu na którym działa usługa. Port powinien być unikalny dla każdej usługi w kapsułce. Wewnątrz kapsułki usługi komunikują się po **localhost**.

:bulb: Kontenery umieszczane w jednej kapsułce zwykle mają zastosowanie w specjalistycznych przypadkach.

Jednaj najczęściej wykorzystywanym scenariuszem jest umieszczanie jednego kontenera w kapsułce a kapsułki komunikują się pomiędzy sobą w wykorzystaniem sieci, Odwołując się do siebie poprzez przydzielone adresy IP.

Często do kapsułki oprócz kontenera aplikacji (app container) dodawany jest osobny kontener siatki (mesh container) który zajmuję się operacjami związanymi z obsługą sieci jak np. szyfrowanie ruchu.

Kapsułka zawsze jest przypisana do jednego węzła (node).

Kapsułki posiadają cykl życia, gdy ulegają awarii powoływane są nowe, każda nowa kapsułka otrzymuje nowy adres IP. Dlatego poszczególne kapsułki komunikują się ze sabą poprzez `SVC` który pełni jednocześnie funkcję **load balancer**.

**SVC** może odwoływać się poprzez etykiety dodane do kapsułek. Etykiet może być kilka. Możemy w ten sposób wdrażać np. aktualizacje kroczące.

Ze względu na to że pody posiadają cykl życia nie łączymy się do nich bezpośrednio, ich adresy IP się zmieniają gdy nowe powoływane są do życia

### Polecenia

```bash
# Tworzenie poda
kubectl apply -f pod.yml

# Pobranie informacji
kubectl get pods
kubectl get pods --watch
# Dodatkowe informacje
kubectl get pods -o wide
# Informacje o określonym pod
kubectl describe pods <podName>

# Usuwanie namespace
kubectl get all
kubectl get all -n <Namespace>
kubectl delete <name>

# Usuwanie poda
kubectl delete pod <podName>
kubectl delete pod -f <fileName>

```

## API

Dostęp do wszystkich składowych i usług klastra odbywa się poprzez jego API. API można przyrównać do katalogu dostępnych funkcji z definicjami tego jak każdy z nich działa.

Odwoływanie się do poszczególnych obiektów oraz pól tego obiektu definiujemy w manifeście, pliku YML. Pola mają podany typ danych jaki mogą przyjmować.

W pliku wymagane jest podanie wersji API z której korzystamy oraz obiektu który będziemy konfigurować.

Każde pole obiektu posiada typ danych który możemy do niego przypisać (integer, boolean, lub typy wbudowane np. LabelSelector)

Obiekt `Deployment`

```YML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

Do komunikacji z serwerem API wykorzystujemy `kubectl` komunikacja przebiega poprzez protokół HTTP

## Instalacja i konfiguracja


## NOTATKI

> Przekierowanie portu z poda
:exclamation: Działa na localhost na serwerze już nie

```bash
kubectl port-forward pods/hello-pod 8080:8080 -n default
```

Udostępnienie portu w sposób imperatywny poprzez stworzenie serwisu który mo dostęp do poda
Tworzy węzeł dostępu w klastrze

```bash
kubectl expose pod hello-pod --name=svc --target-port=8080 --type=NodePort
```
