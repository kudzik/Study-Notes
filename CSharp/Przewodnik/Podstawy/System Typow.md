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

Typ `string` to tak naprawdę tablica znaków o typie `char`. Tekst zawarty w zmiennej typu string zawsze otoczony jest `""`. Jest to typ referencyjny ale zachowuję się podobnie do typu wartościowego. Jest niezmienny więc aby go zmodyfikować musimy ponownie przypisać go do zmiennej.
