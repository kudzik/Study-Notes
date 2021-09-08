# 20210821_CSharp_The_Big_Picture

Pluralsight

C# jest językiem

- Przystępnym
- Silnie typowanym
- Odporny na zamiany (w znaczeniu korzystania z bibliotek dołączonych)
- Zorientowany obiektowo, ale wspierający podejście funkcyjne
- Jest OpenSource
- Jest multiplatformowy

## GetType

[GetType - MS Docs](https://docs.microsoft.com/pl-pl/dotnet/api/system.object.gettype?view=net-5.0)

## Aggregate

[Enumerable.Aggregate - MS Docs](https://docs.microsoft.com/pl-pl/dotnet/api/system.linq.enumerable.aggregate?view=net-5.0#main)

```C#
// Ilość liczb parzystych
var numbers = new int[] { 2, 3, 4, 5 };
var CountAddNumbers = numbers.Aggregate(0, (total, next) => next % 2 == 0 ? total + 1 : total);
System.Console.WriteLine(CountAddNumbers);

// Suma
var SumNumber = numbers.Aggregate(0, (total, next) => total + next);
System.Console.WriteLine(SumNumber);

// Porównuje ilość znaków w nazwie i zwraca dłuższą
string[] fruits = { "apple", "mango", "orange", "passionfruit", "grape" };
string longestName =
    fruits.Aggregate("banana",
                    (longest, next) =>
                        next.Length > longest.Length ? next : longest,
                    // Zwraca końcowy wynik wielkimi literami
                    fruit => fruit.ToUpper());
```

## Właściwości (properties)

```C#
// Właściwości służą do walidacji ustawianych lub zwracanych parametrów
// Metoda ToString() nadpisana przy pomocy wyrażenia lambda
public class Point
{
    // init - ustawia wartość podczas tworzenia obiektu klasy, po przypisaniu wartości nie można zmienić z zewnątrz.
    public int X { get; init; }
    public int Y { get; init; }

    public override string ToString() => $"Point X: {X} Y:{Y}";
}
```

Ustawienie properties w zależności od podanej daty

```C#
enum Generation { BabyBoomer, GenX, Millennial, GenZ, GenA }

class Person
{
    public int BirthYear { get; set; }

    public Generation Generation =>
        BirthYear switch
        {
            (>= 1946) and (<= 1964) => Generation.BabyBoomer,
            (>= 1965) and (<= 1980) => Generation.GenX,
            (>= 1981) and (<= 1996) => Generation.Millennial,
            (>= 1997) and (<= 2012) => Generation.GenZ,
            _ => Generation.GenA
        };
}

var p = new Person { BirthYear = 1950 };
System.Console.WriteLine(p.Generation);
```

## try-catch-finally

- `try` próbuje wykonać kod
- `catch` przechwytuje wyjątek gdy wykonanie kodu w bloku `try` się nie powiedzie
- `finally` wykona się zawsze (najczęściej służy do zwalniania zasobów)

Własne klasy które będą mogły generować wyjątki powinny dziedziczyć po interfejsie `Dispose`, konieczna wtedy jest implementacja metody `Dispose`.
Dzięki takiej implementacji możemy używać deklaracji `using` która automatycznie zwolni zasoby po wykonaniu kodu.

```C#
var widget = new Widget();

try
{
    widget.DoSomething();
}
finally
{
    widget.Dispose();
}

class Widget
{
    public void DoSomething()
    {
        // ....
    }

    public void CleanUp()
    {
        throw new NotImplementedException();
    }
}
```

Własne klasy które będą mogły generować wyjątki powinny dziedziczyć po interfejsie `IDisposable`, konieczna wtedy jest implementacja metody `Dispose`.
Dzięki takiej implementacji możemy używać deklaracji `using` która automatycznie zwolni zasoby po wykonaniu kodu.

```C#
class Widget : IDisposable
{
    public void DoSomething()
    {
        // ....
    }

    public void Dispose()
    {
        throw new NotImplementedException();
    }
}

// Bez konieczności stosowania bloku `finally`
using (var w = new Widget())
{
    w.DoSomething();
}
```
