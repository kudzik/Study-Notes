# Wzorzec repozytorium

Pośredniczy pomiędzy domeną a warstwami odwzorowania danych przy użyciu interfejsu podobnego do kolekcji w celu uzyskania dostępu do obiektów domeny.

Repozytorium ma znajdować się pomiędzy warstwą dostępu do danych, a domeną aplikacji i zachowywać się jak kolekcja, czyli dostarczać możliwość zapisywania, pobierania i usuwania obiektów.

Pozwala oddzielić kod od konkretnej technologii przechowywania danych. Kod aplikacji nie powinien mieć informacji o sposobie działania logiki która za ten dostęp jest odpowiedzialna (jak otwierać pliki, lub jak wykonywać zapytania HTTP). Repozytorium udostępnia dane jako obiekty C#. Możemy utworzyć kilka repozytoriów które będą odpowiadały za różne typy udostępnianych danych (CSV Repository, SQL Repository, JSON Repository), z wszystkim repozytoriami połączenie powinno odbywać się przez wspólny interfejs.

```markdown
                                |- Service Repository - WebService (JSON)
- |Application| - |Interface| - |- CSV Repository - Text File (CSV)
                                |- SQL Repository - SQL Database

```

Repozytoria mogą być różnych typów np. **CRUD** i powinny implementować wspólny interfejs z definicjami metod.

Interfejs powinien mieć tylko operacje wykorzystywane przez klientów, jeśli jest to tylko odczyt nie ma potrzeby korzystania z pozostałych metod takich jak update, create czy delete.
