# Obsługa wyjątków

## Try-catch-finally

- `try` próbuje wykonać kod
- `catch` przechwytuje wyjątek gdy wykonanie kodu w bloku `try` się nie powiedzie
- `finally` wykona się zawsze (najczęściej służy do zwalniania zasobów)

## Własne klasy generujące wyjątki

Własne klasy które będą mogły generować wyjątki powinny dziedziczyć po interfejsie `Dispose`, konieczna wtedy jest implementacja metody `Dispose`.
Dzięki takiej implementacji możemy używać deklaracji `using` która automatycznie zwolni zasoby po wykonaniu kodu.

[Własne klasy obsługujące wyjątki](https://gist.github.com/kudzik/b3238de6d6808c9faced7bb18248dbc1)
