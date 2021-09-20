# System typów języka CSharp

C# Jest językiem silnie typowanym. Oznacza to że wszystkie używane **zmienne** muszą posiadać przypisany typ a **wyrażenia** muszą konkretny typ zwracać, kompilator pilnuje aby podczas ich używania ich typ pozostał bez zmian. Bezpieczeństwo typów zapewnia bezpieczeństwo i chroni przed przypadkowymi błędami. Określając typ deklarujemy rozmiar pamięci jaki będzie dla niego zarezerwowany i czy będzie przechowywany na **stercie** czy na **stosie**. Każdy typ posiada zakres wartości jaki może przyjmować i określa możliwe do wykonania na mim operacje.

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

## Metody

Parametry do metody mogą być przekazywane przez **wartość** lub przez **referencje**.

- Przez wartość

Metoda otrzymuje tylko kopię i nie pracuje na oryginalnych wartościach. Pop wykonaniu działań nie zmieni nic w przekazanych parametrach.

- Przez referencje

Do metody przekazywane są referencje (adresy do wartości), więc jeśli wykonamy jakieś operacje to oryginalne wartości zostaną zmienione