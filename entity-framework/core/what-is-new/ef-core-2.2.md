---
title: Co nowego w EF Core 2.2 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417448"
---
# <a name="new-features-in-ef-core-22"></a>Nowe funkcje w EF Core 2.2

## <a name="spatial-data-support"></a>Obsługa danych przestrzennych

Dane przestrzenne mogą być używane do reprezentowania fizycznej lokalizacji i kształtu obiektów.
Wiele baz danych może natywnie przechowywać, indeksować i badać dane przestrzenne.
Typowe scenariusze obejmują wykonywanie zapytań dotyczących obiektów w danej odległości i testowanie, czy wielokąt zawiera daną lokalizację.
EF Core 2.2 obsługuje teraz pracę z danymi przestrzennymi z różnych baz danych przy użyciu typów z biblioteki [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).

Obsługa danych przestrzennych jest implementowana jako seria pakietów rozszerzeń specyficznych dla dostawcy.
Każdy z tych pakietów współtworzy mapowania dla typów i metod NTS oraz odpowiednich typów przestrzennych i funkcji w bazie danych.
Takie rozszerzenia dostawcy są teraz dostępne dla [SQL Server,](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/) [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)i [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](https://www.npgsql.org/)).
Typy przestrzenne mogą być używane bezpośrednio z [dostawcą ef core w pamięci](xref:core/providers/in-memory/index) bez dodatkowych rozszerzeń.

Po zainstalowaniu rozszerzenia dostawcy można dodać właściwości obsługiwanych typów do jednostek. Przykład:

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
```

Następnie można utrwalać jednostki z danymi przestrzennymi:

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```

I można wykonywać kwerendy bazy danych na podstawie danych przestrzennych i operacji:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Aby uzyskać więcej informacji na temat tej funkcji, zobacz [dokumentację typów przestrzennych](xref:core/modeling/spatial).

## <a name="collections-of-owned-entities"></a>Zbiory podmiotów będących własnością

EF Core 2.0 dodał możliwość modelowania własności w skojarzeniach jeden do jednego.
EF Core 2.2 rozszerza możliwość wyrażania własności do skojarzeń jeden do wielu.
Własność pomaga ograniczyć sposób, w jaki jednostki są używane.

Na przykład jednostki należące do państwa:

- Może być wyświetlany tylko we właściwościach nawigacji innych typów jednostek.
- Są automatycznie ładowane i mogą być śledzone tylko przez DbContext obok ich właściciela.

W relacyjnych bazach danych kolekcje należące do właścicieli są mapowane do oddzielnych tabel od właściciela, podobnie jak zwykłe skojarzenia jeden do wielu.
Jednak w bazach danych zorientowanych na dokumenty planujemy zagnieżdżanie jednostek będących własnością (w posiadanych kolekcjach lub odwołaniach) w ramach tego samego dokumentu co właściciel.

Za pomocą tej funkcji można korzystać, wywołując nowy interfejs API OwnsMany():

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Aby uzyskać więcej informacji, zobacz [zaktualizowaną dokumentację jednostek należących](xref:core/modeling/owned-entities#collections-of-owned-types)do państwa .

## <a name="query-tags"></a>Znaczniki zapytań

Ta funkcja upraszcza korelację zapytań LINQ w kodzie z wygenerowanymi zapytaniami SQL przechwyconymi w dziennikach.

Aby skorzystać ze znaczników kwerend, należy dodawać adnotacje do kwerendy LINQ przy użyciu nowej metody TagWith().
Korzystanie z kwerendy przestrzennej z poprzedniego przykładu:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Ta kwerenda LINQ będzie produkować następujące dane wyjściowe SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Aby uzyskać więcej informacji, zobacz [dokumentację znaczników kwerendy](xref:core/querying/tags).
