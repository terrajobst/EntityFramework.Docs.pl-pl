---
title: What's new in EF Core 2.1 — EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: f67f2e695d269e2dde11d396f9a67fd137600f56
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489406"
---
# <a name="new-features-in-ef-core-21"></a>Nowe funkcje programu EF Core 2.1

Oprócz licznych poprawkach błędów i małych ulepszenia wydajności i funkcjonalności programu EF Core 2.1 zawiera niektóre istotne nowe funkcje:

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem
EF Core zawiera teraz konieczne bloków konstrukcyjnych dla każdego, kto Tworzenie klas jednostek, które mogą ładować swoje właściwości nawigacji na żądanie. Utworzyliśmy również nowy pakiet Microsoft.EntityFrameworkCore.Proxies, który wykorzystuje te bloki konstrukcyjne do produkcji proxy powolne ładowanie klas, w oparciu o co najmniej modyfikacji klas jednostek (na przykład klasy z właściwości nawigacji wirtualnego).

Odczyt [sekcji na ładowanie z opóźnieniem](xref:core/querying/related-data#lazy-loading) Aby uzyskać więcej informacji na ten temat.

## <a name="parameters-in-entity-constructors"></a>Parametry w konstruktorach jednostki
Tworzenie jednostek, które przyjmują parametry w ich konstruktory mogę umożliwiliśmy jako jeden z bloków konstrukcyjnych wymagane do załadowania z opóźnieniem. Parametry można użyć do dodania wartości właściwości, delegatów powolne ładowanie i usług.

Odczyt [sekcji jednostki konstruktora z parametrami](xref:core/modeling/constructors) Aby uzyskać więcej informacji na ten temat.

## <a name="value-conversions"></a>Konwersje wartości
Do tej pory programu EF Core może mapować tylko właściwości typów natywnie obsługiwane przez dostawcę podstawowej bazy danych. Wartości zostały skopiowane i z powrotem między kolumnami i właściwości bez przetwarzania. Począwszy od programu EF Core 2.1 konwersji wartości można zastosować do przekształcania wartości z kolumn przed są stosowane do właściwości i na odwrót. Mamy wiele konwersje, które mogą być stosowane zgodnie z Konwencją, zgodnie z potrzebami, a także interfejsu API jawnego konfiguracji, który umożliwia rejestrowanie niestandardowe konwersje między kolumnami i właściwości. Zastosowanie tej funkcji, należą:

- Przechowywanie typów wyliczeniowych w postaci ciągów
- Mapowanie niepodpisane liczby całkowite z programem SQL Server
- Automatyczne szyfrowanie i odszyfrowywanie wartości właściwości

Odczyt [sekcji konwersji wartości](xref:core/modeling/value-conversions) Aby uzyskać więcej informacji na ten temat.  

## <a name="linq-groupby-translation"></a>Tłumaczenie LINQ GroupBy
Przed wersją 2.1 w wersji EF Core operatorów GroupBy LINQ zawsze oceniono w pamięci. Obsługujemy teraz go tłumaczenia klauzuli SQL GROUP BY w najbardziej typowe przypadki.

W tym przykładzie przedstawiono zapytanie z GroupBy używany do obliczania różne funkcje agregujące:

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
Za pomocą nowej wersji będzie możliwe zapewnienie początkowe dane, aby wypełnić bazę danych. W odróżnieniu od w EF6, wstępne wypełnianie danych jest skojarzony typ jednostki jako część konfiguracji modelu. Następnie migracje EF Core można automatycznie obliczyć co Wstawianie, aktualizowanie lub usuwanie potrzebę operacji mają być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.

Na przykład można użyj go do skonfigurowania danych inicjatora dla wpisu w `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Odczyt [sekcji na wstępne wypełnianie danych](xref:core/modeling/data-seeding) Aby uzyskać więcej informacji na ten temat.  

## <a name="query-types"></a>Typy zapytań
Modelu platformy EF Core mogą teraz zawierać typy zapytania. Inaczej niż w przypadku typów jednostek, typy zapytań nie kluczy zdefiniowane i nie można wstawić, usunąć lub zaktualizować (oznacza to, że są one tylko do odczytu), ale mogą być zwrócone bezpośrednio przez zapytania. Scenariusze użycia dla typów zapytań, należą:

- Mapowanie do widoków bez kluczy podstawowych
- Mapowania tabel bez kluczy podstawowych
- Mapowanie do zapytań zdefiniowanych w modelu
- Służy jako typ zwracany dla `FromSql()` zapytań

Odczyt [sekcji na typy zapytań](xref:core/modeling/query-types) Aby uzyskać więcej informacji na ten temat.

## <a name="include-for-derived-types"></a>Zawierają typy pochodne
Zostanie on teraz jest to możliwe, aby określić właściwości nawigacji tylko wobec podczas pisania wyrażeń dla typów pochodnych `Include` metody. Silnie typizowaną wersję `Include`, obsługujemy za pomocą jawnego rzutowania lub `as` operatora. Firma Microsoft obsługuje teraz również odwołuje się do nazwy właściwości nawigacji zdefiniowany dla typów pochodnych w wersję ciągu `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Odczyt [sekcji Dołącz z typami pochodnymi](xref:core/querying/related-data#include-on-derived-types) Aby uzyskać więcej informacji na ten temat.

## <a name="systemtransactions-support"></a>Obsługa System.Transactions
Dodaliśmy możliwość współpracy z System.Transactions funkcje, takie jak TransactionScope. Będzie to działać dla platformy .NET Core i .NET Framework podczas korzystania z dostawcy baz danych, które go obsługują.

Odczyt [sekcji System.Transactions](xref:core/saving/transactions#using-systemtransactions) Aby uzyskać więcej informacji na ten temat.

## <a name="better-column-ordering-in-initial-migration"></a>Lepsze kolejność kolumn w początkowej migracji
Na podstawie opinii klientów Zaktualizowaliśmy migracji można wstępnie wygenerować kolumn dla tabel w tej samej kolejności, ponieważ właściwości są deklarowane w klasach. Należy pamiętać, programem EF Core nie można zmienić kolejności po dodaniu nowych elementów członkowskich po utworzeniu początkowego tabeli.

## <a name="optimization-of-correlated-subqueries"></a>Optymalizacja skorelowany podzapytań
Ulepszyliśmy nasze translacji zapytania, aby uniknąć wykonywania "N + 1" zapytania SQL w wielu typowych scenariuszy, w których użycie właściwości nawigacji w projekcji prowadzi do łączenie danych z zapytania katalogu głównego przy użyciu danych z skorelowane podzapytanie. Optymalizacja, wymagane jest buforowanie wyników z podzapytanie, a firma Microsoft wymaga, aby zmodyfikować zapytanie, aby zdecydować się na nowe zachowanie.

Na przykład następujące zapytanie zwykle pobiera przetłumaczone na jednej kwerendzie dla klientów, a także N (gdzie "N" oznacza liczbę klientów zwrócił) oddzielnych zapytań dla zleceń:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Jeśli dołączysz `ToList()` w odpowiednim miejscu, wskazujesz, że buforowanie jest odpowiednia dla zamówienia, które umożliwiają optymalizację:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Należy pamiętać, że to zapytanie będzie tłumaczona tylko dwa kwerendy SQL: jeden dla klientów i kolejny zamówień.

## <a name="owned-attribute"></a>Atrybut [należące do firmy]

Teraz jest możliwa do skonfigurowania [posiadane typy jednostek](xref:core/modeling/owned-entities) , po prostu Dodawanie adnotacji do typu z `[Owned]` a następnie sprawdzając, czy jednostka właściciel jest dodawany do modelu:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Narzędzie wiersza polecenia dotnet-ef zawarte w zestawie SDK programu .NET Core

_Dotnet ef_ polecenia są teraz częścią programu .NET Core SDK, w związku z tym nie będzie konieczne użycie DotNetCliToolReference w projekcie, aby można było użyć migracje lub tworzenia szkieletu DbContext z istniejącej bazy danych.

Zobacz sekcję dotyczącą [instalowania narzędzi](xref:core/miscellaneous/cli/dotnet#installing-the-tools) Aby uzyskać więcej informacji o sposobie włączania narzędzia wiersza polecenia dla różnych wersji zestawu SDK programu .NET Core i programem EF Core.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Pakiet Microsoft.EntityFrameworkCore.Abstractions
Nowy pakiet zawiera atrybuty i interfejsy, które można użyć w swoich projektach, aby wzbogacić funkcji EF Core bez zależna od programu EF Core jako całości. Na przykład atrybut [posiadane] i interfejs ILazyLoader znajdują się w tym miejscu.

## <a name="state-change-events"></a>Zdarzenia zmiany stanu

Nowe `Tracked` i `StateChanged` zdarzenia `ChangeTracker` może być użyty do zapisu logikę, która reaguje na jednostki, wprowadzając kontekstu DbContext lub zmianę ich stanu.

## <a name="raw-sql-parameter-analyzer"></a>Nieprzetworzone analizatora parametru SQL

Nowy analizator kodu jest dołączana do programu EF Core, który wykrywa potencjalnie niebezpieczną użycia interfejsów API raw SQL, takie jak `FromSql` lub `ExecuteSqlCommand`. Na przykład dla następującego zapytania, zobaczysz ostrzeżenie ponieważ _minAge_ nie jest sparametryzowane:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Zgodności dostawcy bazy danych

Zaleca się używać programu EF Core 2.1 z dostawcami, które zostały zaktualizowane lub co najmniej przetestowany w celu pracy z programem EF Core 2.1.

> [!TIP]
> Jeśli okaże się wszelkie nieoczekiwane niezgodności dowolne wysłać nowych funkcji lub jeśli chcesz przesłać opinię na nich, zgłoś go za pomocą [nasze narzędzia do śledzenia błędów](https://github.com/aspnet/EntityFrameworkCore/issues/new).
