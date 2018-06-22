---
title: Klient programu vs. Ocenianie Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054158"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="0e90e-102">Klient programu vs. Ocena serwera</span><span class="sxs-lookup"><span data-stu-id="0e90e-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="0e90e-103">Program Entity Framework Core obsługuje części zapytania są oceniane na kliencie i części przekazywanej do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0e90e-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="0e90e-104">Jest dostawca bazy danych, aby określić części zapytania, które zostanie obliczone w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0e90e-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="0e90e-105">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="0e90e-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="0e90e-106">Ocena klienta</span><span class="sxs-lookup"><span data-stu-id="0e90e-106">Client evaluation</span></span>

<span data-ttu-id="0e90e-107">W poniższym przykładzie metoda pomocnika służy do normalizacji adresów URL dla blogów, z którego są zwracane z bazy danych programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e90e-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="0e90e-108">Ponieważ dostawca programu SQL Server nie ma wgląd implementowania tej metody, nie jest możliwe umożliwiło SQL.</span><span class="sxs-lookup"><span data-stu-id="0e90e-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="0e90e-109">Innych aspektów zapytania są oceniane w bazie danych, ale przekazywanie zwróconego `URL` za pomocą tej metody jest wykonywana na kliencie.</span><span class="sxs-lookup"><span data-stu-id="0e90e-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="0e90e-110">Wyłączanie oceny klienta</span><span class="sxs-lookup"><span data-stu-id="0e90e-110">Disabling client evaluation</span></span>

<span data-ttu-id="0e90e-111">Podczas oceny klienta może być bardzo przydatne, w niektórych przypadkach, które mogą skutkować niską wydajnością.</span><span class="sxs-lookup"><span data-stu-id="0e90e-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="0e90e-112">Należy wziąć pod uwagę następujące zapytanie, gdy metoda pomocnika teraz jest używana w filtrze.</span><span class="sxs-lookup"><span data-stu-id="0e90e-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="0e90e-113">Ponieważ to nie można wykonać w bazie danych, wszystkie dane są pobierane do pamięci, a następnie filtr jest stosowany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="0e90e-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="0e90e-114">W zależności od ilości danych i ilości danych jest odfiltrowany może to spowodować niską wydajnością.</span><span class="sxs-lookup"><span data-stu-id="0e90e-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="0e90e-115">Domyślnie EF Core będzie rejestrować ostrzeżenie podczas oceny klienta jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="0e90e-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="0e90e-116">Zobacz [rejestrowanie](../miscellaneous/logging.md) Aby uzyskać więcej informacji o wyświetlaniu dane wyjściowe rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="0e90e-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="0e90e-117">Zachowanie można zmienić podczas oceny klienta throw lub nie rób.</span><span class="sxs-lookup"><span data-stu-id="0e90e-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="0e90e-118">Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` Jeśli używasz platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e90e-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
