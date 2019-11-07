---
title: Co nowego w EF Core 2,2 — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656189"
---
# <a name="new-features-in-ef-core-22"></a>Nowe funkcje w EF Core 2,2

## <a name="spatial-data-support"></a>Obsługa danych przestrzennych

Dane przestrzenne mogą służyć do reprezentowania fizycznej lokalizacji i kształtu obiektów.
Wiele baz danych może natywnie przechowywać, indeksować i wykonywać zapytania dotyczące danych przestrzennych.
Typowe scenariusze obejmują zapytania dotyczące obiektów w ramach danej odległości i testowania, jeśli Wielokąt zawiera daną lokalizację.
EF Core 2,2 obsługuje teraz pracę z danymi przestrzennymi z różnych baz danych przy użyciu typów z biblioteki [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (nkty przerwania).

Obsługa danych przestrzennych jest implementowana jako seria pakietów rozszerzeń specyficznych dla dostawcy.
Każdy z tych pakietów tworzy mapowania dla typów i metod NKTY przerwania oraz odpowiednie typy i funkcje przestrzenne w bazie danych.
Takie rozszerzenia dostawcy są teraz dostępne dla [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)i [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](https://www.npgsql.org/)).
Typów przestrzennych można używać bezpośrednio z [EF Core dostawcy w pamięci](xref:core/providers/in-memory/index) bez dodatkowych rozszerzeń.

Po zainstalowaniu rozszerzenia dostawcy można dodać do jednostek właściwości obsługiwanych typów. Na przykład:

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

Można wykonywać zapytania bazy danych na podstawie danych przestrzennych i operacji:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Aby uzyskać więcej informacji na temat tej funkcji, zobacz [dokumentację typów przestrzennych](xref:core/modeling/spatial).

## <a name="collections-of-owned-entities"></a>Kolekcje jednostek będących własnością

EF Core 2,0 dodaliśmy możliwość modelowania własności w skojarzeniach jeden-do-jednego.
EF Core 2,2 rozszerza możliwość wyrażania własności na skojarzenia jeden-do-wielu.
Własność pomaga ograniczyć sposób używania jednostek.

Na przykład posiadane jednostki:

- Mogą być wyświetlane tylko na właściwościach nawigacji innych typów jednostek.
- Są ładowane automatycznie i mogą być śledzone tylko przez DbContext wraz z ich właścicielem.

W relacyjnych bazach danych kolekcje będące własnością są mapowane na oddzielne tabele od właściciela, podobnie jak regularne skojarzenia jeden-do-wielu.
Ale w bazach danych opartych na dokumentach planujemy zagnieżdżać jednostki posiadane (w kolekcjach będących własnością lub odwołania) w tym samym dokumencie co właściciel.

Możesz użyć tej funkcji, wywołując Nowy interfejs API OwnsMany ():

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Aby uzyskać więcej informacji, zobacz [dokumentację informacji o zaktualizowanych jednostkach](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Tagi zapytań

Ta funkcja upraszcza korelację zapytań LINQ w kodzie z wygenerowanymi zapytaniami SQL przechwytywanymi w dziennikach.

Aby skorzystać z tagów zapytania, można dodać adnotacje do zapytania LINQ przy użyciu nowej metody TagWith ().
Użycie zapytania przestrzennego z poprzedniego przykładu:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

To zapytanie LINQ zwróci następujące dane wyjściowe SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Aby uzyskać więcej informacji, zobacz [dokumentację tagów zapytań](xref:core/querying/tags).
