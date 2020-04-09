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
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="bda67-102">Nowe funkcje w EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="bda67-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="bda67-103">Obsługa danych przestrzennych</span><span class="sxs-lookup"><span data-stu-id="bda67-103">Spatial data support</span></span>

<span data-ttu-id="bda67-104">Dane przestrzenne mogą być używane do reprezentowania fizycznej lokalizacji i kształtu obiektów.</span><span class="sxs-lookup"><span data-stu-id="bda67-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="bda67-105">Wiele baz danych może natywnie przechowywać, indeksować i badać dane przestrzenne.</span><span class="sxs-lookup"><span data-stu-id="bda67-105">Many databases can natively store, index, and query spatial data.</span></span>
<span data-ttu-id="bda67-106">Typowe scenariusze obejmują wykonywanie zapytań dotyczących obiektów w danej odległości i testowanie, czy wielokąt zawiera daną lokalizację.</span><span class="sxs-lookup"><span data-stu-id="bda67-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="bda67-107">EF Core 2.2 obsługuje teraz pracę z danymi przestrzennymi z różnych baz danych przy użyciu typów z biblioteki [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).</span><span class="sxs-lookup"><span data-stu-id="bda67-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="bda67-108">Obsługa danych przestrzennych jest implementowana jako seria pakietów rozszerzeń specyficznych dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="bda67-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="bda67-109">Każdy z tych pakietów współtworzy mapowania dla typów i metod NTS oraz odpowiednich typów przestrzennych i funkcji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bda67-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="bda67-110">Takie rozszerzenia dostawcy są teraz dostępne dla [SQL Server,](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/) [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/)i [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (z [projektu Npgsql](https://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="bda67-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="bda67-111">Typy przestrzenne mogą być używane bezpośrednio z [dostawcą ef core w pamięci](xref:core/providers/in-memory/index) bez dodatkowych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="bda67-111">Spatial types can be used directly with the [EF Core in-memory provider](xref:core/providers/in-memory/index) without additional extensions.</span></span>

<span data-ttu-id="bda67-112">Po zainstalowaniu rozszerzenia dostawcy można dodać właściwości obsługiwanych typów do jednostek.</span><span class="sxs-lookup"><span data-stu-id="bda67-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="bda67-113">Przykład:</span><span class="sxs-lookup"><span data-stu-id="bda67-113">For example:</span></span>

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

<span data-ttu-id="bda67-114">Następnie można utrwalać jednostki z danymi przestrzennymi:</span><span class="sxs-lookup"><span data-stu-id="bda67-114">You can then persist entities with spatial data:</span></span>

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

<span data-ttu-id="bda67-115">I można wykonywać kwerendy bazy danych na podstawie danych przestrzennych i operacji:</span><span class="sxs-lookup"><span data-stu-id="bda67-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="bda67-116">Aby uzyskać więcej informacji na temat tej funkcji, zobacz [dokumentację typów przestrzennych](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="bda67-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span>

## <a name="collections-of-owned-entities"></a><span data-ttu-id="bda67-117">Zbiory podmiotów będących własnością</span><span class="sxs-lookup"><span data-stu-id="bda67-117">Collections of owned entities</span></span>

<span data-ttu-id="bda67-118">EF Core 2.0 dodał możliwość modelowania własności w skojarzeniach jeden do jednego.</span><span class="sxs-lookup"><span data-stu-id="bda67-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="bda67-119">EF Core 2.2 rozszerza możliwość wyrażania własności do skojarzeń jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="bda67-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="bda67-120">Własność pomaga ograniczyć sposób, w jaki jednostki są używane.</span><span class="sxs-lookup"><span data-stu-id="bda67-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="bda67-121">Na przykład jednostki należące do państwa:</span><span class="sxs-lookup"><span data-stu-id="bda67-121">For example, owned entities:</span></span>

- <span data-ttu-id="bda67-122">Może być wyświetlany tylko we właściwościach nawigacji innych typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="bda67-122">Can only ever appear on navigation properties of other entity types.</span></span>
- <span data-ttu-id="bda67-123">Są automatycznie ładowane i mogą być śledzone tylko przez DbContext obok ich właściciela.</span><span class="sxs-lookup"><span data-stu-id="bda67-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="bda67-124">W relacyjnych bazach danych kolekcje należące do właścicieli są mapowane do oddzielnych tabel od właściciela, podobnie jak zwykłe skojarzenia jeden do wielu.</span><span class="sxs-lookup"><span data-stu-id="bda67-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="bda67-125">Jednak w bazach danych zorientowanych na dokumenty planujemy zagnieżdżanie jednostek będących własnością (w posiadanych kolekcjach lub odwołaniach) w ramach tego samego dokumentu co właściciel.</span><span class="sxs-lookup"><span data-stu-id="bda67-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="bda67-126">Za pomocą tej funkcji można korzystać, wywołując nowy interfejs API OwnsMany():</span><span class="sxs-lookup"><span data-stu-id="bda67-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="bda67-127">Aby uzyskać więcej informacji, zobacz [zaktualizowaną dokumentację jednostek należących](xref:core/modeling/owned-entities#collections-of-owned-types)do państwa .</span><span class="sxs-lookup"><span data-stu-id="bda67-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="bda67-128">Znaczniki zapytań</span><span class="sxs-lookup"><span data-stu-id="bda67-128">Query tags</span></span>

<span data-ttu-id="bda67-129">Ta funkcja upraszcza korelację zapytań LINQ w kodzie z wygenerowanymi zapytaniami SQL przechwyconymi w dziennikach.</span><span class="sxs-lookup"><span data-stu-id="bda67-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="bda67-130">Aby skorzystać ze znaczników kwerend, należy dodawać adnotacje do kwerendy LINQ przy użyciu nowej metody TagWith().</span><span class="sxs-lookup"><span data-stu-id="bda67-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="bda67-131">Korzystanie z kwerendy przestrzennej z poprzedniego przykładu:</span><span class="sxs-lookup"><span data-stu-id="bda67-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="bda67-132">Ta kwerenda LINQ będzie produkować następujące dane wyjściowe SQL:</span><span class="sxs-lookup"><span data-stu-id="bda67-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="bda67-133">Aby uzyskać więcej informacji, zobacz [dokumentację znaczników kwerendy](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="bda67-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span>
