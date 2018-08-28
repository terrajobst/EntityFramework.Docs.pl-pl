---
title: Klient programu vs. Ocena serwera — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 78f8d9576748a725634665f915def80b5a13820c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997880"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="c3c24-102">Klient programu vs. Ocena serwera</span><span class="sxs-lookup"><span data-stu-id="c3c24-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="c3c24-103">Entity Framework Core obsługuje części zapytania Trwa obliczanie na urządzeniu klienckim i części wypychania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c3c24-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="c3c24-104">Jest dostawca bazy danych, aby określić części zapytania, które zostanie obliczone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c3c24-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="c3c24-105">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c3c24-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="c3c24-106">Oceny klienta</span><span class="sxs-lookup"><span data-stu-id="c3c24-106">Client evaluation</span></span>

<span data-ttu-id="c3c24-107">W poniższym przykładzie metoda pomocnika służy do standaryzacji adresy URL dla blogów, z którego są zwracane z bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c3c24-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="c3c24-108">Ponieważ dostawca programu SQL Server nie wgląd w jaki sposób ta metoda jest implementowana, nie jest możliwe tłumaczenie SQL.</span><span class="sxs-lookup"><span data-stu-id="c3c24-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="c3c24-109">Wszystkie inne aspekty zapytania są oceniane w bazie danych, ale przekazywanie zwracanego `URL` za pośrednictwem tej metody jest wykonywane na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="c3c24-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="disabling-client-evaluation"></a><span data-ttu-id="c3c24-110">Wyłączanie oceny klienta</span><span class="sxs-lookup"><span data-stu-id="c3c24-110">Disabling client evaluation</span></span>

<span data-ttu-id="c3c24-111">Oceny klienta mogą być bardzo przydatne, w niektórych przypadkach go może spowodować obniżenie wydajności.</span><span class="sxs-lookup"><span data-stu-id="c3c24-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="c3c24-112">Należy wziąć pod uwagę następujące zapytanie, której metody pomocnika teraz jest używane w filtrze.</span><span class="sxs-lookup"><span data-stu-id="c3c24-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="c3c24-113">Ponieważ to nie może zostać wykonana w bazie danych, wszystkie dane są pobierane do pamięci, a następnie filtr jest stosowany na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="c3c24-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="c3c24-114">W zależności od ilości danych, a ilość tych danych jest odfiltrowana może to spowodować spadek wydajności.</span><span class="sxs-lookup"><span data-stu-id="c3c24-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="c3c24-115">Domyślnie program EF Core będzie rejestrować ostrzeżenie podczas oceny klienta jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="c3c24-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="c3c24-116">Zobacz [rejestrowania](../miscellaneous/logging.md) więcej informacji na temat wyświetlania danych wyjściowych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="c3c24-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="c3c24-117">Można zmienić zachowanie w przypadku oceny klienta throw lub nic nie rób.</span><span class="sxs-lookup"><span data-stu-id="c3c24-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="c3c24-118">Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` korzystania z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3c24-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
