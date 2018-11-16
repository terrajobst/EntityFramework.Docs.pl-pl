---
title: What's new in EF Core 2.2 — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688808"
---
# <a name="new-features-in-ef-core-22"></a>Nowe funkcje programu EF Core 2.2

## <a name="spatial-data-support"></a>Obsługa danych przestrzennych

Dane przestrzenne może służyć do reprezentowania fizycznej lokalizacji i kształt obiektów.
Wiele baz danych natywnie można przechowywać, indeksu i wykonywanie zapytań o dane przestrzenne. Typowe scenariusze obejmują tworzenie zapytań dotyczących obiektów w określonej odległości i testowania, jeśli wielokąta zawiera danej lokalizacji.
Obsługuje teraz EF Core 2.2 z Praca z danymi przestrzennymi z różnymi bazami danych przy użyciu typów z [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) biblioteki (NTS).

Obsługa danych przestrzennych jest zaimplementowana jako szereg pakietów rozszerzeń właściwe dla dostawcy.
Każda z tych pakietów wspiera mapowania dla typów nkty przerwania i metod oraz odpowiadające typy przestrzenne i funkcji w bazie danych.
Takie rozszerzenia dostawcy są teraz dostępne dla [programu SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), i [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](http://www.npgsql.org/)).
Typów przestrzennych mogą być używane bezpośrednio z [dostawcy w pamięci programu EF Core](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) bez dodatkowych rozszerzeń.

Po zainstalowaniu rozszerzenia dostawcy można dodać właściwości obsługiwane typy do jednostek. Na przykład:

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

Można następnie zachować jednostek o dane przestrzenne:

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
I można wykonywać zapytania bazy danych na podstawie danych przestrzennych i operacje:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Aby uzyskać więcej informacji na temat tej funkcji, zobacz [dokumentacji typów przestrzennych](xref:core/modeling/spatial). 

## <a name="collections-of-owned-entities"></a>Kolekcje jednostki należące do firmy

EF Core 2.0 dodanie do użytkowania model w jeden do jednego skojarzenia.
EF Core 2.2 rozszerza możliwości express własność skojarzenia jeden do wielu.
Własność pomaga ograniczyć, jak jednostki są używane.

Na przykład należących do jednostki:
- Może się pojawić tylko dla właściwości nawigacji innych typów jednostek. 
- Są ładowane automatycznie i może być śledzone tylko przez DbContext wraz z ich właścicielem.

Relacyjne bazy danych kolekcji należące do firmy są mapowane do oddzielnych tabel z jego właściciela, podobnie jak regularne skojarzenia jeden do wielu.
Jednak w bazach danych korzystający z dokumentów, planujemy zagnieździć należących do podmiotów (w kolekcji należące do firmy lub odwołania) w obrębie tego samego dokumentu jako właściciel.

Można użyć tej funkcji przez wywołanie metody nowy interfejs API OwnsMany():

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Aby uzyskać więcej informacji, zobacz [zaktualizowane należących do jednostek dokumentacja](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Tagi kwerendy

Ta funkcja ułatwia korelacja zapytania LINQ w kodzie za pomocą wygenerowanego zapytań SQL przechwycone w dziennikach.

Aby móc korzystać z kwerendy, tagi, dodawać adnotacje zapytania LINQ za pomocą nowej metody TagWith().
Przy użyciu zapytań przestrzennych z poprzedniego przykładu:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

To zapytanie LINQ generuje następujące dane wyjściowe SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Aby uzyskać więcej informacji, zobacz [zapytania tagów dokumentacji](xref:core/querying/tags). 