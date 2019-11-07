---
title: Co nowego w EF Core 2,1 — EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654845"
---
# <a name="new-features-in-ef-core-21"></a>Nowe funkcje w EF Core 2,1

Oprócz licznych poprawek i niewielkich ulepszeń funkcjonalnych i wydajności EF Core 2,1 obejmuje niektóre atrakcyjne nowe funkcje:

## <a name="lazy-loading"></a>Ładowanie z opóźnieniem

EF Core teraz zawiera niezbędne bloki konstrukcyjne dla każdego do tworzenia klas jednostek, które mogą ładować właściwości nawigacji na żądanie. Utworzyliśmy również nowy pakiet Microsoft. EntityFrameworkCore. proxy, który korzysta z tych bloków konstrukcyjnych, aby utworzyć klasy proxy z opóźnieniem, oparte na minimalnych modyfikacjach klas jednostek (na przykład klasy z wirtualnymi właściwościami nawigacji).

Zapoznaj się z [sekcją dotyczącą ładowania z opóźnieniem](xref:core/querying/related-data#lazy-loading) , aby uzyskać więcej informacji na temat tego tematu.

## <a name="parameters-in-entity-constructors"></a>Parametry w konstruktorach jednostek

Jako jeden z wymaganych bloków konstrukcyjnych dla ładowania z opóźnieniem włączono tworzenie jednostek, które przyjmują parametry w konstruktorach. Za pomocą parametrów można wstrzyknąć wartości właściwości, delegatów ładowania z opóźnieniem i usług.

Przeczytaj [sekcję w konstruktorze jednostek z parametrami](xref:core/modeling/constructors) , aby uzyskać więcej informacji na temat tego tematu.

## <a name="value-conversions"></a>Konwersje wartości

Do tej pory EF Core mogą mapować tylko właściwości typów natywnie obsługiwanych przez bazowego dostawcę bazy danych. Wartości zostały skopiowane z powrotem między kolumnami i właściwościami bez żadnego przekształcenia. Począwszy od EF Core 2,1, konwersje wartości można zastosować do przekształcenia wartości uzyskanych z kolumn przed ich zastosowaniem do właściwości i na odwrót. Mamy kilka konwersji, które mogą być stosowane do Konwencji w razie potrzeby, a także jawny interfejs API konfiguracji, który umożliwia rejestrowanie konwersji niestandardowych między kolumnami i właściwościami. Niektóre aplikacje tej funkcji są następujące:

- Przechowywanie tekstów stałych jako ciągów
- Mapowanie liczb całkowitych bez znaku przy użyciu SQL Server
- Automatyczne szyfrowanie i odszyfrowywanie wartości właściwości

Zapoznaj się z [sekcją konwersje wartości](xref:core/modeling/value-conversions) , aby uzyskać więcej informacji na temat tego tematu.  

## <a name="linq-groupby-translation"></a>Tłumaczenie LINQ GroupBy

Przed wersjami 2,1, w EF Core operator GroupBy LINQ zawsze będzie oceniany w pamięci. Teraz obsługujemy tłumaczenie tego kodu do klauzuli GROUP BY w większości typowych przypadków.

Ten przykład pokazuje zapytanie z atrybutem GroupBy używanym do obliczania różnych funkcji agregujących:

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
          Avg = g.Average(o => o.Amount)
        });
```

Odpowiednie tłumaczenie SQL wygląda następująco:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Wstępne wypełnianie danych

Dzięki nowej wersji będzie możliwe dostarczenie początkowych danych w celu wypełniania bazy danych. W przeciwieństwie do EF6, wypełnianie danych jest skojarzone z typem jednostki w ramach konfiguracji modelu. Następnie EF Core migracji mogą automatycznie obliczyć operacje wstawiania, aktualizowania lub usuwania, które należy zastosować podczas uaktualniania bazy danych do nowej wersji modelu.

Przykładowo można użyć tego do skonfigurowania danych inicjatora dla wpisu w `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Zapoznaj się z [sekcją dotyczącą](xref:core/modeling/data-seeding) umieszczania danych, aby uzyskać więcej informacji na temat tego tematu.  

## <a name="query-types"></a>Typy zapytań

Model EF Core może teraz zawierać typy zapytań. W przeciwieństwie do typów jednostek, typy zapytań nie mają zdefiniowanych kluczy i nie można ich wstawiać, usuwać ani aktualizować (oznacza to, że są tylko do odczytu), ale mogą być zwracane bezpośrednio przez zapytania. Niektóre scenariusze użycia dla typów zapytań są następujące:

- Mapowanie do widoków bez kluczy podstawowych
- Mapowanie do tabel bez kluczy podstawowych
- Mapowanie na zapytania zdefiniowane w modelu
- Obsługa jako typ zwracany dla zapytań `FromSql()`

Aby uzyskać więcej informacji na temat tego tematu, zapoznaj się z [sekcją dotyczącą typów zapytań](xref:core/modeling/keyless-entity-types) .

## <a name="include-for-derived-types"></a>Uwzględnij dla typów pochodnych

Teraz można określić właściwości nawigacji zdefiniowane tylko w typach pochodnych podczas pisania wyrażeń dla metody `Include`. W przypadku silnie wpisanej wersji `Include`obsługujemy użycie jawnego rzutowania lub operatora `as`. Teraz obsługujemy również odwoływanie się do nazw właściwości nawigacji zdefiniowanej w typach pochodnych w wersji ciągu `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Zapoznaj się z [sekcją zawiera typy pochodne](xref:core/querying/related-data#include-on-derived-types) , aby uzyskać więcej informacji na temat tego tematu.

## <a name="systemtransactions-support"></a>Obsługa system. Transactions

Dodaliśmy możliwość pracy z funkcjami system. Transactions, takimi jak element TransactionScope. Będzie to możliwe w przypadku .NET Framework i .NET Core przy użyciu dostawców baz danych, które go obsługują.

Zapoznaj się z [sekcją system. Transactions](xref:core/saving/transactions#using-systemtransactions) , aby uzyskać więcej informacji na temat tego tematu.

## <a name="better-column-ordering-in-initial-migration"></a>Lepsza kolejność kolumn podczas migracji początkowej

Na podstawie opinii klientów Zaktualizowaliśmy migracje w celu wstępnego wygenerowania kolumn dla tabel w tej samej kolejności, w której właściwości są zadeklarowane w klasach. Należy pamiętać, że EF Core nie może zmienić kolejności po dodaniu nowych członków po początkowej operacji tworzenia tabeli.

## <a name="optimization-of-correlated-subqueries"></a>Optymalizacja skorelowanych podkwerend

Ulepszono nasze tłumaczenie zapytań, aby uniknąć wykonywania zapytań SQL "N + 1" w wielu typowych scenariuszach, w których użycie właściwości nawigacji w projekcji prowadzi do dołączania danych z zapytania głównego z danymi z skorelowanego podzapytania. Optymalizacja wymaga buforowania wyników z podzapytania i wymaga modyfikacji zapytania, aby wybrać nowe zachowanie.

Na przykład następujące zapytanie jest zwykle tłumaczone na jedno zapytanie dla klientów i N (gdzie "N" jest liczbą zwracanych klientów) oddzielnymi zapytania dla zamówień:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Dołączając `ToList()` w odpowiednim miejscu, oznacza to, że buforowanie jest odpowiednie dla zamówień, które umożliwiają optymalizację:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Należy zauważyć, że to zapytanie zostanie przetłumaczone tylko na dwie zapytania SQL: jeden dla klientów i następny jeden dla zamówień.

## <a name="owned-attribute"></a>[Własność] — atrybut

Teraz można skonfigurować [własne typy jednostek](xref:core/modeling/owned-entities) poprzez po prostu adnotację typu z `[Owned]`, a następnie upewniając się, że jednostka właściciela została dodana do modelu:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Narzędzie wiersza polecenia dotnet-EF zawarte w zestaw .NET Core SDK

Polecenia _dotnet-EF_ są teraz częścią zestaw .NET Core SDK, dlatego nie trzeba już używać DotNetCliToolReference w projekcie, aby można było używać migracji lub do tworzenia szkieletu DbContext z istniejącej bazy danych.

Zapoznaj się z sekcją [Instalowanie narzędzi,](xref:core/miscellaneous/cli/dotnet#installing-the-tools) Aby uzyskać więcej informacji na temat włączania narzędzi wiersza polecenia dla różnych wersji zestaw .NET Core SDK i EF Core.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Pakiet Microsoft. EntityFrameworkCore. Abstracts

Nowy pakiet zawiera atrybuty i interfejsy, których można używać w projektach, aby wyrównać funkcje EF Core bez konieczności EF Core jako całości. Na przykład, atrybut [własność] i interfejs ILazyLoader znajdują się tutaj.

## <a name="state-change-events"></a>Zdarzenia zmiany stanu

Nowe `Tracked` i `StateChanged` zdarzenia w `ChangeTracker` mogą służyć do pisania logiki, która reaguje na jednostki wchodzące w obiekt DbContext lub zmieniając ich stan.

## <a name="raw-sql-parameter-analyzer"></a>Nieprzetworzony Analizator parametrów SQL

Do EF Core jest dołączany nowy analizator kodu, który wykrywa potencjalnie niebezpieczne użycia interfejsów API RAW-SQL, takich jak `FromSql` lub `ExecuteSqlCommand`. Na przykład dla poniższego zapytania zobaczysz ostrzeżenie, ponieważ _minAge_ nie jest sparametryzowane:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Zgodność dostawcy bazy danych

Zaleca się używanie EF Core 2,1 z dostawcami, którzy zostali zaktualizowani lub co najmniej przetestowani do pracy z EF Core 2,1.

> [!TIP]
> Jeśli okaże się, że wystąpiły nieoczekiwane niezgodność lub jakiekolwiek problemy w nowych funkcjach, lub jeśli masz opinię na ich temat, zgłoś je za pomocą [naszego narzędzia do śledzenia problemów](https://github.com/aspnet/EntityFrameworkCore/issues/new).
