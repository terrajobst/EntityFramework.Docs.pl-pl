---
title: Co to jest nowa w programie EF 2.1 Core - EF Core
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: bb1e691e0f22bd36467d58c02bde22c63067207e
ms.sourcegitcommit: fcaeaf085171dfb5c080ec42df1d1df8dfe204fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="new-features-in-ef-core-21"></a>Nowe funkcje w programie EF Core 2.1
> [!NOTE]  
> Ta wersja jest dostępny w wersji zapoznawczej.

Oprócz wiele małych ulepszeń i poprawek usterek produktu więcej niż stu EF Core 2.1 zawiera kilka nowych funkcji:

## <a name="lazy-loading"></a>powolne ładowanie
Podstawowe EF zawiera teraz konieczne bloków konstrukcyjnych dla każdego do tworzenia klas jednostek, które można załadować ich właściwości nawigacji na żądanie. Również utworzono nowy pakiet, Microsoft.EntityFrameworkCore.Proxies, który wykorzystuje te bloki konstrukcyjne do utworzenia serwera proxy opóźnionego ładowania klas minimalny zestaw na podstawie zmodyfikowane klasy jednostki (np. klasy z właściwości nawigacji wirtualnego).

Odczyt [sekcję opóźnionego ładowania](xref:core/querying/related-data#lazy-loading) Aby uzyskać więcej informacji dotyczących tego tematu.

## <a name="parameters-in-entity-constructors"></a>Parametry w konstruktorach jednostki
Jako jeden z wymaganych bloków konstrukcyjnych dla ładowania opóźnionego możemy włączono tworzenie jednostek, które pobierają parametry w ich konstruktorów. Można wstawić wartości właściwości, delegatów opóźnionego ładowania i usługi mogą używać parametrów.

Odczyt [sekcję na temat jednostek konstruktora o parametrach](xref:core/modeling/constructors) Aby uzyskać więcej informacji dotyczących tego tematu.

## <a name="value-conversions"></a>Konwersje wartości
Do tej pory EF Core można mapują tylko właściwości typów obsługiwane przez dostawcę podstawowej bazy danych. Wartości zostały skopiowane i z powrotem między kolumnami i właściwości bez przetwarzania. W programie EF Core 2.1, konwersji wartości można zastosować do przekształcenia wartości z kolumn przed ich zastosowaniem do właściwości i na odwrót. Mamy wiele konwersje, które mogą być stosowane przez Konwencję w razie potrzeby, jak również jawne konfiguracji interfejsu API, który umożliwia rejestrowanie niestandardowe konwersje między kolumnami i właściwości. Niektóre stosowania tej funkcji to:

- Przechowywanie wyliczenia w postaci ciągów
- Mapowanie podpisane liczby całkowite z programem SQL Server
- Automatycznego szyfrowania i odszyfrowywania wartości właściwości

Odczyt [sekcję konwersji wartości](xref:core/modeling/value-conversions) Aby uzyskać więcej informacji dotyczących tego tematu.  

## <a name="linq-groupby-translation"></a>Tłumaczenie LINQ GroupBy
Przed wersji 2.1, w podstawowej EF operator GroupBy LINQ jest zawsze oceniane w pamięci. Obsługujemy teraz tłumaczenia go do klauzuli SQL GROUP BY w typowych przypadkach.

Ten przykład zawiera zapytanie z GroupBy używanych do obliczenia różne funkcje agregujące:

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

Odpowiednie tłumaczenia SQL wygląda następująco:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Wstępne wypełnianie danych
Wraz z wydaniem nowej będzie możliwe zapewnienie początkowej danych do wypełniania bazy danych. W odróżnieniu od do EF6, wstępne wypełnianie danych jest przypisany do typu jednostki jako część konfiguracji modelu. Następnie EF Core migracji można automatycznie obliczyć co insert, update lub delete potrzebę operacje można zastosować podczas uaktualniania bazy danych do nowej wersji modelu.

Na przykład można użyj go do skonfigurowania danych inicjatora dla żądania Post w `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

Odczyt [sekcję na temat wstępnego wypełniania danych](xref:core/modeling/data-seeding) Aby uzyskać więcej informacji dotyczących tego tematu.  

## <a name="query-types"></a>Typy zapytań
Model EF Core teraz może zawierać typy zapytań. Inaczej niż w przypadku typów jednostek, typy zapytań nie zostały zdefiniowane na nich klucze i nie można wstawić, usunięte lub zaktualizowane (tj. są tylko do odczytu), ale może być zwracany bezpośrednio przez zapytania. Niektóre scenariusze użycia dotyczące typy zapytań to:

- Mapowanie do widoków bez kluczy podstawowych
- Mapowanie do tabel bez kluczy podstawowych
- Mapowanie do zapytań, zdefiniowanego w modelu
- Służy jako typ zwracany dla `FromSql()` zapytań

Odczyt [sekcję na temat typów zapytań](xref:core/modeling/query-types) Aby uzyskać więcej informacji dotyczących tego tematu.

## <a name="include-for-derived-types"></a>Uwzględnij typy pochodne
Będzie ona teraz można określić właściwości nawigacji zdefiniowana tylko na typów pochodnych, gdy tworzenie wyrażeń na potrzeby `Include` metody. Dla silnie typizowaną wersję `Include`, firma Microsoft obsługuje za pomocą jawnego rzutowania lub `as` operatora. Firma Microsoft obsługuje teraz również odwołuje się do nazwy właściwości nawigacji zdefiniowanej w typach pochodnych w ciągu `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Odczyt [sekcję Include z typami pochodnymi](xref:core/querying/related-data#include-on-derived-types) Aby uzyskać więcej informacji dotyczących tego tematu.

## <a name="systemtransactions-support"></a>Obsługa System.Transactions
Dodano możliwość korzystania z funkcji System.Transactions, takich jak element TransactionScope. To będzie działać na .NET Framework i .NET Core przy użyciu dostawcy bazy danych, które obsługują tę.

Odczyt [sekcję System.Transactions](xref:core/saving/transactions#using-systemtransactions) Aby uzyskać więcej informacji dotyczących tego tematu.

## <a name="better-column-ordering-in-initial-migration"></a>Lepsze kolejność kolumn w początkowej migracji
Na podstawie opinii klientów, zostały zaktualizowane migracje początkowo Generowanie kolumn w tabelach w tej samej kolejności jak właściwości są zadeklarowane w klasach. Należy pamiętać, EF Core nie można zmienić kolejności podczas dodawania nowych elementów członkowskich po utworzeniu początkowej tabeli.

## <a name="optimization-of-correlated-subqueries"></a>Optymalizacja podzapytania skorelowane
Firma Microsoft ulepszyła naszych Translacja zapytania, aby uniknąć wykonywania "N + 1" zapytania SQL w wielu typowych scenariuszy, w których użycie właściwości nawigacji w projekcji prowadzi do łączenie danych z zapytania głównego z danymi z podzapytania skorelowane. Optymalizacja wymaga wyników buforowania tworzą podzapytanie i wymagamy, należy zmodyfikować zapytanie, aby zdecydować się na nowe zachowanie.

Na przykład poniższe zapytanie zwykle pobiera przetłumaczyć jednej kwerendzie dla klientów, a także N (gdzie "N" oznacza liczbę klientów zwrócił) oddzielne zapytania dla zleceń:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

W tym `ToList()` w odpowiednim miejscu, należy wskazać, że buforowanie jest odpowiednie dla zleceń, które umożliwiają optymalizację:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Należy pamiętać, że to zapytanie zostanie przekonwertowana na stronę tylko dwa zapytania SQL: jeden dla klientów i kolejnego dla zleceń.

## <a name="ownedattribute"></a>OwnedAttribute

Obecnie istnieje możliwość skonfigurowania [należące do typów jednostek](xref:core/modeling/owned-entities) przez po prostu Dodawanie adnotacji do typu z `[Owned]` , a następnie sprawdzając, czy jednostka właściciela jest dodawane do modelu:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="database-provider-compatibility"></a>Zgodność dostawcy bazy danych

Podstawowe EF 2.1 zaprojektowano tak, aby był zgodny z dostawcy bazy danych utworzone dla EF Core 2.0. Podczas gdy niektóre funkcje opisane powyżej (np. wartość konwersje) wymagają zaktualizowanej dostawcy, innych użytkowników (np. podczas ładowania opóźnionego) uaktywni się z istniejącymi dostawcami.

> [!TIP]
> Jeśli okaże się żadnym nieoczekiwany niezgodności lub dowolnym wystawiania w nowych funkcji lub jeśli masz opinię na nich, zgłoś go za pomocą [naszych tracker problem](https://github.com/aspnet/EntityFrameworkCore/issues/new).
