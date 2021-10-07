# Notatki

## Typ Dynamic

:bulb: Zmienne dynamic są sprawdzane w trakcie wykonywania programu podczas gdy var sprawdzany jest w trakcie kompilacji.

| var                                                                                                                                                                                                      | dynamic                                                                                                                                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| statyczne typowanie – typ zmiennej deklarowanej jest określany przez kompilator w trakcie kompilowania kodu                                                                                              | dynamiczne typowanie – typ zmiennej deklarowanej jest określany przez kompilator w trakcie wykonywania                                                                                                                         |
| zmienna musi być zainicjowana w trakcie deklaracji                                                                                                                                                       | zmienna nie musi zainicjowana w trakcie deklaracji                                                                                                                                                                             |
| błędy są wyłapywane w trakcie kompilacji – kompilator wie jakiego typu jest zmienna, dodatkowo posiada informacje o metodach czy właściwościach dlatego jest w stanie zwrócić błędy w trakcie kompilacji | błędy są wyłapywane w trakcie wykonywania – dopiero w trakcie wykonywania kompilator uzyska informacje o rodzaju zadeklarowanej zmiennej dlatego też wszystkie ewentualne błędy będą wyłapywane w trakcie wykonywania programu |
| Intellisense jest dostępny dla zmiennej zdeklarowanej jako var                                                                                                                                           | Intellisense jest niedostępny dla zmiennej zdeklarowanej jako dynamic ponieważ jej typ zostanie poznany dopiero w trakcie wykonywania                                                                                          |
| var nie może zostać użyty jako parametr zwracany                                                                                                                                                         | dynamic może zostać użyty jako parametr zwracany                                                                                                                                                                               |
| nie możemy przypisać wartości null                                                                                                                                                                       | wartość null może zostać przypisana                                                                                                                                                                                            |
[Source]("https://www.plukasiewicz.net/Artykuly/Var_vs_dynamic")

## Serializacja i deserializacja obiektów

### JSON

Nazwy obiektów JSON zawierają małe litery, aby dostosować ich wielkość do obiektów C# możemy zastosować opcje

```csharp
JsonSerializerOptions jsonSerializerOptions = new JsonSerializerOptions { PropertyNameCaseInsensitive=true};

// Deserializacja, zmiana odpowiedzi w JSON na obiekty C#
JsonSerializer.Deserialize<IEnumerable<Person>>(response, options);
```

## Konfiguracja bez ponownego kompilowania

[MS Docs](https://docs.microsoft.com/pl-pl/troubleshoot/dotnet/csharp/store-custom-information-config-file)

W Visual Studio możemy dodać plik konfiguracyjny z którego konfiguracja będzie wczytywana podczas uruchamiania programu, nie musimy ponownie kompilować kodu.
Realizowane jest to za pomocą wstrzykiwania zależności i interfejsu `IConfiguration` oraz wczytania zamiennej zawartej w pliku.

W aplikacjach konsolowych konieczne jest dodanie zestawu Nuget `System.Configuration.ConfigurationManager`.

Plik konfiguracji zostanie wyeksportowany do katalogu `bin`, plik możemy edytować zmiany w pliku zostaną odzwierciedlone w aplikacji bez ponownej kompilacji.

