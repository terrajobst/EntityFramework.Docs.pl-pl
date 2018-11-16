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
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="e7af1-102">Nowe funkcje programu EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="e7af1-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="e7af1-103">Obsługa danych przestrzennych</span><span class="sxs-lookup"><span data-stu-id="e7af1-103">Spatial data support</span></span>

<span data-ttu-id="e7af1-104">Dane przestrzenne może służyć do reprezentowania fizycznej lokalizacji i kształt obiektów.</span><span class="sxs-lookup"><span data-stu-id="e7af1-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="e7af1-105">Wiele baz danych natywnie można przechowywać, indeksu i wykonywanie zapytań o dane przestrzenne.</span><span class="sxs-lookup"><span data-stu-id="e7af1-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="e7af1-106">Typowe scenariusze obejmują tworzenie zapytań dotyczących obiektów w określonej odległości i testowania, jeśli wielokąta zawiera danej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="e7af1-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="e7af1-107">Obsługuje teraz EF Core 2.2 z Praca z danymi przestrzennymi z różnymi bazami danych przy użyciu typów z [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) biblioteki (NTS).</span><span class="sxs-lookup"><span data-stu-id="e7af1-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="e7af1-108">Obsługa danych przestrzennych jest zaimplementowana jako szereg pakietów rozszerzeń właściwe dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="e7af1-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="e7af1-109">Każda z tych pakietów wspiera mapowania dla typów nkty przerwania i metod oraz odpowiadające typy przestrzenne i funkcji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e7af1-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="e7af1-110">Takie rozszerzenia dostawcy są teraz dostępne dla [programu SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), i [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](http://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="e7af1-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](http://www.npgsql.org/)).</span></span>
<span data-ttu-id="e7af1-111">Typów przestrzennych mogą być używane bezpośrednio z [dostawcy w pamięci programu EF Core](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) bez dodatkowych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="e7af1-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="e7af1-112">Po zainstalowaniu rozszerzenia dostawcy można dodać właściwości obsługiwane typy do jednostek.</span><span class="sxs-lookup"><span data-stu-id="e7af1-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="e7af1-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e7af1-113">For example:</span></span>

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

<span data-ttu-id="e7af1-114">Można następnie zachować jednostek o dane przestrzenne:</span><span class="sxs-lookup"><span data-stu-id="e7af1-114">You can then persist entities with spatial data:</span></span>

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
<span data-ttu-id="e7af1-115">I można wykonywać zapytania bazy danych na podstawie danych przestrzennych i operacje:</span><span class="sxs-lookup"><span data-stu-id="e7af1-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="e7af1-116">Aby uzyskać więcej informacji na temat tej funkcji, zobacz [dokumentacji typów przestrzennych](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="e7af1-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="e7af1-117">Kolekcje jednostki należące do firmy</span><span class="sxs-lookup"><span data-stu-id="e7af1-117">Collections of owned entities</span></span>

<span data-ttu-id="e7af1-118">EF Core 2.0 dodanie do użytkowania model w jeden do jednego skojarzenia.</span><span class="sxs-lookup"><span data-stu-id="e7af1-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="e7af1-119">EF Core 2.2 rozszerza możliwości express własność skojarzenia jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="e7af1-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="e7af1-120">Własność pomaga ograniczyć, jak jednostki są używane.</span><span class="sxs-lookup"><span data-stu-id="e7af1-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="e7af1-121">Na przykład należących do jednostki:</span><span class="sxs-lookup"><span data-stu-id="e7af1-121">For example, owned entities:</span></span>
- <span data-ttu-id="e7af1-122">Może się pojawić tylko dla właściwości nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="e7af1-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="e7af1-123">Są ładowane automatycznie i może być śledzone tylko przez DbContext wraz z ich właścicielem.</span><span class="sxs-lookup"><span data-stu-id="e7af1-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="e7af1-124">Relacyjne bazy danych kolekcji należące do firmy są mapowane do oddzielnych tabel z jego właściciela, podobnie jak regularne skojarzenia jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="e7af1-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="e7af1-125">Jednak w bazach danych korzystający z dokumentów, planujemy zagnieździć należących do podmiotów (w kolekcji należące do firmy lub odwołania) w obrębie tego samego dokumentu jako właściciel.</span><span class="sxs-lookup"><span data-stu-id="e7af1-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="e7af1-126">Można użyć tej funkcji przez wywołanie metody nowy interfejs API OwnsMany():</span><span class="sxs-lookup"><span data-stu-id="e7af1-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="e7af1-127">Aby uzyskać więcej informacji, zobacz [zaktualizowane należących do jednostek dokumentacja](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="e7af1-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="e7af1-128">Tagi kwerendy</span><span class="sxs-lookup"><span data-stu-id="e7af1-128">Query tags</span></span>

<span data-ttu-id="e7af1-129">Ta funkcja ułatwia korelacja zapytania LINQ w kodzie za pomocą wygenerowanego zapytań SQL przechwycone w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="e7af1-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="e7af1-130">Aby móc korzystać z kwerendy, tagi, dodawać adnotacje zapytania LINQ za pomocą nowej metody TagWith().</span><span class="sxs-lookup"><span data-stu-id="e7af1-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="e7af1-131">Przy użyciu zapytań przestrzennych z poprzedniego przykładu:</span><span class="sxs-lookup"><span data-stu-id="e7af1-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="e7af1-132">To zapytanie LINQ generuje następujące dane wyjściowe SQL:</span><span class="sxs-lookup"><span data-stu-id="e7af1-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="e7af1-133">Aby uzyskać więcej informacji, zobacz [zapytania tagów dokumentacji](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="e7af1-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 