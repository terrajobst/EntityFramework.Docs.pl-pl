---
title: Co nowego w EF Core 2.1 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: ba3a26bcd76cd0b9615b13f32456e7280afe533a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417483"
---
# <a name="new-features-in-ef-core-21"></a>Nowe funkcje w EF Core 2.1

Oprócz wielu poprawek błędów oraz małych ulepszeń funkcjonalnych i wydajności, EF Core 2.1 zawiera kilka atrakcyjnych nowych funkcji:

## <a name="lazy-loading"></a>Z opóźnieniem załadunku

EF Core zawiera teraz niezbędne bloki konstrukcyjne dla każdego do autora klas jednostek, które można załadować ich właściwości nawigacji na żądanie. Utworzyliśmy również nowy pakiet, Microsoft.EntityFrameworkCore.Proxies, który wykorzystuje te bloki konstrukcyjne do tworzenia klas proxy z opóźnieniem ładowania na podstawie minimalnie zmodyfikowanych klas jednostek (na przykład klas z właściwościami nawigacji wirtualnej).

Przeczytaj [sekcję o z opóźnieniem ładowania,](xref:core/querying/related-data#lazy-loading) aby uzyskać więcej informacji na ten temat.

## <a name="parameters-in-entity-constructors"></a>Parametry w konstruktorach jednostek

Jako jeden z wymaganych bloków konstrukcyjnych dla ładowania z opóźnieniem, włączyliśmy tworzenie jednostek, które przyjmują parametry w ich konstruktorów. Za pomocą parametrów można wstrzyknąć wartości właściwości, z opóźnieniem ładowania delegatów i usług.

Przeczytaj [sekcję konstruktora jednostek z parametrami,](xref:core/modeling/constructors) aby uzyskać więcej informacji na ten temat.

## <a name="value-conversions"></a>Konwersje wartości

Do tej pory EF Core może mapować tylko właściwości typów natywnie obsługiwanych przez dostawcę podstawowej bazy danych. Wartości zostały skopiowane tam iz powrotem między kolumnami i właściwości bez przekształcenia. Począwszy od EF Core 2.1, konwersje wartości mogą być stosowane do przekształcania wartości uzyskanych z kolumn, zanim zostaną zastosowane do właściwości i odwrotnie. Mamy szereg konwersji, które mogą być stosowane przez konwencję w razie potrzeby, a także jawny interfejs API konfiguracji, który umożliwia rejestrowanie konwersji niestandardowych między kolumnami i właściwościami. Niektóre z zastosowań tej funkcji są:

- Przechowywanie wyliczenia jako ciągów
- Mapowanie niepodpisanych danych całkowitych za pomocą programu SQL Server
- Automatyczne szyfrowanie i odszyfrowywanie wartości właściwości

Przeczytaj [sekcję dotyczącą konwersji wartości,](xref:core/modeling/value-conversions) aby uzyskać więcej informacji na ten temat.  

## <a name="linq-groupby-translation"></a>LINQ GroupPrzezdanie

Przed wersją 2.1, w EF Core GroupBy LINQ operator zawsze będą oceniane w pamięci. Obecnie obsługujemy tłumaczenie go do klauzuli SQL GROUP BY w większości typowych przypadków.

W tym przykładzie przedstawiono kwerendę z GroupBy używane do obliczania różnych funkcji agregacji:

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

Dzięki nowej wersji będzie można podać wstępne dane do wypełniania bazy danych. W przeciwieństwie do EF6, dane wysiewu jest skojarzony z typem jednostki jako część konfiguracji modelu. Następnie ef core migracje można automatycznie obliczyć, co wstawić, zaktualizować lub usunąć operacje muszą być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.

Na przykład można użyć tego do skonfigurowania danych `OnModelCreating`źródłowych dla wpisu w:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Przeczytaj [sekcję dotyczącą rozmieszczania danych,](xref:core/modeling/data-seeding) aby uzyskać więcej informacji na ten temat.  

## <a name="query-types"></a>Typy zapytań

Model EF Core może teraz zawierać typy zapytań. W przeciwieństwie do typów jednostek typy zapytań nie mają kluczy zdefiniowanych na nich i nie mogą być wstawiane, usuwane lub aktualizowane (oznacza to, że są tylko do odczytu), ale mogą być zwracane bezpośrednio przez kwerendy. Niektóre scenariusze użycia dla typów zapytań są:

- Mapowanie do widoków bez kluczy podstawowych
- Mapowanie do tabel bez kluczy podstawowych
- Mapowanie do zapytań zdefiniowanych w modelu
- Służąc jako typ `FromSql()` zwracany dla kwerend

Przeczytaj [sekcję dotyczącą typów kwerend,](xref:core/modeling/keyless-entity-types) aby uzyskać więcej informacji na ten temat.

## <a name="include-for-derived-types"></a>Uwzględnij dla typów pochodnych

Będzie teraz można określić właściwości nawigacji zdefiniowane tylko dla typów `Include` pochodnych podczas pisania wyrażeń dla metody. Dla silnie typizowanej `Include`wersji , obsługujemy przy użyciu `as` jawnego rzutu lub operatora. Obsługujemy również odwoływanie się do nazw właściwości nawigacji zdefiniowanych `Include`na typach pochodnych w wersji ciągu:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Przeczytaj [sekcję dołącz do typów pochodnych,](xref:core/querying/related-data#include-on-derived-types) aby uzyskać więcej informacji na ten temat.

## <a name="systemtransactions-support"></a>Obsługa system.transactions

Dodaliśmy możliwość pracy z system.transactions funkcje, takie jak TransactionScope. Będzie to działać zarówno na .NET Framework i .NET Core podczas korzystania z dostawców bazy danych, które go obsługują.

Przeczytaj [sekcję o System.Transactions,](xref:core/saving/transactions#using-systemtransactions) aby uzyskać więcej informacji na ten temat.

## <a name="better-column-ordering-in-initial-migration"></a>Lepsze zamawianie kolumn w migracji początkowej

Na podstawie opinii klientów zaktualizowaliśmy migracje, aby początkowo generować kolumny dla tabel w tej samej kolejności, co właściwości są deklarowane w klasach. Należy zauważyć, że EF Core nie można zmienić kolejność, gdy nowe elementy członkowskie są dodawane po utworzeniu tabeli początkowej.

## <a name="optimization-of-correlated-subqueries"></a>Optymalizacja skorelowanych podquerii

Ulepszyliśmy nasze tłumaczenie zapytań, aby uniknąć wykonywania zapytań SQL "N + 1" w wielu typowych scenariuszach, w których użycie właściwości nawigacji w projekcji prowadzi do łączenia danych z zapytania głównego z danymi ze skorelowanego podzapytania. Optymalizacja wymaga buforowania wyników z podzapytania i wymagamy, aby zmodyfikować kwerendę, aby wyrazić zgodę na nowe zachowanie.

Na przykład następująca kwerenda zwykle jest tłumaczona na jedną kwerendę dla klientów oraz N (gdzie "N" jest liczbą zwróconych klientów) oddzielne zapytania dla zamówień:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Po `ToList()` uwzględnieniu we właściwym miejscu, należy wskazać, że buforowanie jest odpowiednie dla zamówień, które umożliwiają optymalizację:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Należy zauważyć, że ta kwerenda zostanie przetłumaczona tylko na dwa zapytania SQL: Jeden dla klientów i następny dla zamówień.

## <a name="owned-attribute"></a>[Własnością] atrybut

Teraz można skonfigurować [typy jednostek będących własnością,](xref:core/modeling/owned-entities) po prostu dokonując adnotacji o typie, `[Owned]` a następnie upewniając się, że jednostka właściciela jest dodawana do modelu:

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

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a>Narzędzie wiersza polecenia dotnet-ef zawarte w zestawie SDK rdzenia .NET

Polecenia _dotnet-ef_ są teraz częścią .NET Core SDK, dlatego nie będzie już konieczne użycie dotNetCliToolReference w projekcie, aby móc używać migracji lub szkieletowania DbContext z istniejącej bazy danych.

Zobacz sekcję [dotyczącą instalowania narzędzi,](xref:core/miscellaneous/cli/dotnet#installing-the-tools) aby uzyskać więcej informacji na temat włączania narzędzi wiersza polecenia dla różnych wersji zestawu .NET Core SDK i EF Core.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Pakiet Microsoft.EntityFrameCore.Abstractions

Nowy pakiet zawiera atrybuty i interfejsy, których można użyć w projektach, aby zapalić funkcje EF Core bez uwzględnienia zależności od EF Core jako całości. Na przykład [Owned] atrybut i interfejs ILazyLoader znajdują się tutaj.

## <a name="state-change-events"></a>Zdarzenia zmiany stanu

Nowe `Tracked` `StateChanged` i `ChangeTracker` zdarzenia na może służyć do pisania logiki, która reaguje na jednostki wprowadzające DbContext lub zmiana ich stanu.

## <a name="raw-sql-parameter-analyzer"></a>Analizator parametrów RAW SQL

Nowy analizator kodu jest dołączony do EF Core, który wykrywa potencjalnie niebezpieczne użycie `FromSql` naszych `ExecuteSqlCommand`interfejsów API raw-SQL, takich jak lub . Na przykład dla następującej kwerendy zostanie wyświetlone ostrzeżenie, ponieważ _minAge_ nie jest sparametryzowany:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Zgodność dostawcy bazy danych

Zaleca się, aby używać EF Core 2.1 z dostawcami, które zostały zaktualizowane lub przynajmniej przetestowane do pracy z EF Core 2.1.

> [!TIP]
> Jeśli znajdziesz nieoczekiwaną niezgodność lub jakikolwiek problem w nowych funkcjach lub jeśli masz opinię na ich temat, zgłoś to za pomocą [naszego trackera problemów.](https://github.com/aspnet/EntityFrameworkCore/issues/new)
