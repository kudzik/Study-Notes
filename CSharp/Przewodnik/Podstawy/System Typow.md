# System typów języka CSharp

C# Jest językiem silnie typowanym. Oznacza to że wszystkie używane **zmienne** muszą posiadać przypisany typ a **wyrażenia** muszą konkretny typ zwracać, kompilator pilnuje aby podczas ich używania ich typ pozostał bez zmian. Bezpieczeństwo typów zapewnia bezpieczeństwo i chroni przed przypadkowymi błędami. Określając typ deklarujemy rozmiar pamięci jaki będzie dla niego zarezerwowany i czy będzie przechowywany na **stercie** czy na **stosie**. Każdy typ posiada zakres wartości jaki może przyjmować i określa możliwe do wykonania na mim operacje.

W skrócie możemy powiedzieć że typy wartości przechowują dane a typy referencyjne adres który wskazuję miejsce w którym dane się znajdują.

## Typy wbudowane

Niektóre typy wbudowane:

- bool
- int
- float, double, decimal
- char, string
- byte
- object

Typy posiadają skrócone wersje swoich nazw tzw. aliasy np. typ `int` to tak naprawdę `Int32`.

## Konwersje pomiędzy typami

Z powodu silnego typowania nie możemy bezpośrednio przypisać innego typu do zadeklarowanej wcześniej zmiennej, jeśli nie stracimy danych(precyzji) kompilator sam zmieni typ danych (konwersja niejawna np. z `int` na `double`). Jeśli konwersja może doprowadzić do utraty precyzji trzeba wykonać jawne rzutowanie lub skorzystać z konwersji typów.

## Typy niejawne

Jeśli typ możemy wywnioskować z kontekstu zamiast jawnie go podawać możemy użyć słowa kluczowego `var`.

## Tworzenie i używanie string

Typ `string` to tak naprawdę tablica znaków o typie `char`. Tekst zawarty w zmiennej typu string zawsze otoczony jest `""`. Jest to typ referencyjny ale zachowuję się podobnie do typu wartościowego. Jest niezmienny więc podczas modyfikacji tworzona i przypisywana nowa wartość.

Zmienna typu `string` nie przechowuje wartości tylko **referencje** (adres pamięci w którym znajduje się wartość). Przy przypisaniu wartości do innej zmiennej i jego modyfikacji oryginalny ciąg pozostaje bez zmian.

:bulb: Typy referencyjne przechowywane są na **stercie** a typy wartościowe na **stosie**.

Podczas pracy na string, gdy wartości są często modyfikowane i kopiowane powinniśmy skorzystać z klasy `System.Text.StringBuilder`.

Łączenie string

```Csharp
int year = 2021;
string s1 = "Hello";
string s2 = "World";
string s3 = s1 + s2;
string s4 = string.Concat(s1, s2);
string s5 = "Hello" + year + "World";

// Interpolacja
string hi = $"Hello {s2}";

```

## Wartości null

Gdy referencja zapisana w zmiennej nie wskazuje na żadne miejsce w pamięci ma wartość null. Dzieje sie tak np. gdy tworzymy obiekt który jest tylko zainicjalizowany ale nie stworzymy jeszcze jego instancji za pomocą słowa kluczowego `null`. Sama nazwa zmiennej jest przechowywana na stosie ale wartość na którą wskazuje referencja nie została jeszcze zarezerwowana w pamięci i nie istnieje na stercie.

Wartość null możemy również przypisać do zmiennej referencyjnej której już nie będziemy potrzebować, jeśli do wartości na stercie nie będzie prowadziła żadna referencja zostanie ona zebrana przez **garbage collector** i miejsce w pamięci zostanie odzyskane.

```csharp
Person person; // person ma wartość null

person = new Person() // zarezerwowanie miejsca w pamięci
```

:bulb: Jeśli obiekt ma wartość `null` wszystkie jego składowe mają wartość `null`.

C# Obsługuje typy wartościowe które mogą przyjmować null tzw. `nullable`. Null ma zastosowanie gdy chcemy powiedzieć że coś nie ma wartości, jest to różnica pomiędzy np. wartością zerową.

:bulb: Dzięki `null` możemy odwzorować zawartość bazy danych gdy w danym rekordzie nie posiadamy danych.

O ile typ **int** ma zawsze wartość tak typ zadeklarowany jako nullable `int?` to takiej pewności mieć nie musi, sprawdzić możemy to za pomocą `HasValue`.

```csharp
int? x = 10;
int? y = null;

// Sprawdzenie czy typ nullable ma wartość

if(y.HasValue) {
    //...
}

// Operator sprawdzenia null
// Jeśli y będzie null użyj wartości po prawej stronie
y ?? 12

```

## Garbage Collector

Uwalnia miejsce na stercie gdy zostaje zerwana referencja do niej prowadząca. Zmienna nadal może się znajdować na stercie a zerwanie nastąpiło poprzez przypisanie do niej wartości null.

Możemy go wywołać samodzielnie poprzez `GC.Collect()` ale nie mamy gwarancji że zadziała ponieważ pierwszeństwo w zarządzaniu nim posiada **CLR**

## Metody

Parametry do metody mogą być przekazywane przez **wartość** lub przez **referencje**.

- Przez wartość

Metoda otrzymuje tylko kopię i nie pracuje na oryginalnych wartościach. Pop wykonaniu działań nie zmieni nic w przekazanych parametrach.

- Przez referencje

Do metody przekazywane są referencje (adresy do wartości), więc jeśli wykonamy jakieś operacje to oryginalne wartości zostaną zmienione. Aby przekazać referencję używamy słowa `ref` lub `out`.

:bulb: Do przekazywanego parametru `ref` musi być przypisana wartość, parametr `out` wystarczy że będzie zainicjalizowany. `out` może zostać również wykorzystany do zwrócenia wielu wartości z metody.

### Argumenty nazwane i parametry opcjonalne

Parametry opcjonalne podajemy jako ostatnie.

```CSharp
// Parametry nazwane
Add(a: 3, b:8);

public int Add(int a, int b, int c = 0)
```

### Expressions syntax body

```CSharp
 private static int Add(int v1, int v2, int v3) => v1 + v2 + v3;
```

## Przestrzenie nazw i biblioteki klas bazowych BCL

W Visual studio narzędzie które pozwala przeglądać typy w przestrzeniach nazw `Object Browser` (CTRL+ALT+J)

## Klasy

Klasa to reprezentacja obiektu występującego w świecie rzeczywistym. Jest szablonem który możemy wykorzystać do tworzenia instancji obiektu.

Klasa `Osoba` która zawiera składowe np. `Imię` i `Nazwisko` to szablon, na jej podstawie możemy tworzyć konkretne egzemplarze, które mają nadane imię i nazwisko.

```Csharp
// Klasa szablon
 class Person
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }

// Tworzenie instancji na podstawie klasy
Person Tom = new Person() { FirstName = "Tom", LastName = "Smith" };
Person Bruce = new Person() { FirstName = "Bruce", LastName = "Lee" };
```

Klasa zawiera składowe które reprezentują cechy odwzorowanego obiektu oraz metody charakteryzujące zachowania, ogólnie można przyjąć że właściwości to przymiotniki a metody zawarte w klasie to rzeczowniki.

Składowe klasy:

- Pola / Fields
- Metody / Methods
- Właściwości / Properties
- Zdarzenia / Events

:bulb: Klasy są typami referencyjnymi które są przechowywane na stercie.

### Konstruktor

Podczas tworzenia obiektu pierwszy zostanie wykonany konstruktor, gdy nie zdefiniujemy własnego w klasie zostanie utworzony **konstruktor domyślny**. Konstruktory możemy również przeciążać.

### Właściwości

Działają jak pola klasy, ale dzięki nim możemy wprowadzić dodatkową kontrolę ustawianych i pobieranych wartości. W rzeczywistości to funkcje które mogą weryfikować poprawność danych które są ustawiane i pobierane z klasy. Możemy dzięki temu ukryć logikę a także zadbać o to które pola są możliwe do odczytu lub zapisu z zewnątrz klasy.

### Składowe statyczne

Składowe statyczne w klasie należą do samej klasy a nie do jej instancji, dane zawarte w składowej są dzielone pomiędzy jej wszystkimi instancjami. Możemy się do nich odwołać poprzez `NazwaKlasy.WłaściwośćStatyczna`.

## Dziedziczenie

Dziedziczenie, wraz z hermetyzację i polimorfizmem, jest jedną z trzech podstawowych cech programowania obiektowego. Dziedziczenie umożliwia tworzenie nowych klas, które ponownie, rozszerzają i modyfikują zachowanie zdefiniowane w innych klasach. Klasa, której składowe są dziedziczone, jest nazywana klasą bazową , a klasa dziedzicząca te składowe jest nazywana klasą pochodną. Klasa pochodna może mieć tylko jedną bezpośrednią klasę bazową.

:bulb: Klasa pochodna rozszerza działanie klasy z której dziedziczy, dziedziczenie może zachodzić jeśli spełnimy warunek `jest` (np. **Manager** jest **pracownikiem**)

Jeśli chcemy aby składowa klasy była widziana tylko dla klasy która z niej dziedziczy musimy ustawić dostęp na `protected`.

W klasie pochodnej konstruktor z klasy bazowej wywołujemy za pomocą `:base`.

## Polimorfizm

Pozwala na nadpisanie zachowania z klasy bazowej w klasie pochodnej. Metoda z klasy bazowej może być przesłonięta w klasie która z niej dziedziczy. Nowa implementacja może całkowicie zmieniać działanie nadpisywanej metody.

Metody w klasie bazowej oznaczamy `virtual`, metody w klasie pochodnej oznaczmy `override`

Jeśli w klasie pochodnej metoda nie zostanie nadpisana zostanie użyta metoda z klasy bazowej.

Obiekty klasy pochodnej możemy tworzyć odnosząc sie do typu podstawowego

```csharp
Employee e1 = new Manager();
Employee e1 = new Developer();
```

Może to się przydać w różnych sytuacjach. Przykładowo możemy stworzyć tablicę typu `Employee` umieścić w niej obiekty typu pochodnego i na wszystkich wykonać metodę np. `.Work()`. Użyta zostanie wtedy metoda odpowiednia dla każdego typu jeśli została nadpisana w typie pochodnym.

- Statyczny
- Dynamiczny

## Klasy i metody Abstrakcyjne

Klasy abstrakcyjne to klasy na podstawie których **nie możemy** utworzyć instancji obiektu. Odnoszą się do rzeczywistych obiektów które są abstrakcyjne (nie możemy utworzyć wystąpienia klasy **figura geometryczna**).

Metoda abstrakcyjna musi zostać zaimplementowana w klasie dziedziczącej.

## Interfejsy

Interfejs to kontrakt, zawiera listę składowych bez implementacji które klasa implementująca interfejs musi zaimplementować. Interfejsy podobnie jak klasy abstrakcyjne nie pozwalają na utworzenie ich instancji.

Według konwencji nazwy interfejsów zaczynamy od dużej litery `I` np. `IEnumerable`.

Interfejsy mogą korzystać z polimorfizmu, jeśli klasy implementują ten sam interfejs można stworzyć obiekt typu implementowanego interfejsu.
