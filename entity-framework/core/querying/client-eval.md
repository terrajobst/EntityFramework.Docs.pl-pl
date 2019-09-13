---
title: Klient a Ocena serwera — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921699"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="99321-102">Klient a Ocena serwera</span><span class="sxs-lookup"><span data-stu-id="99321-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="99321-103">Entity Framework Core obsługuje części kwerendy ocenianej na kliencie i części, które są przekazywane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="99321-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="99321-104">Aby określić, które części zapytania będą oceniane w bazie danych, należy do dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="99321-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="99321-105">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="99321-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="99321-106">Ocena klienta</span><span class="sxs-lookup"><span data-stu-id="99321-106">Client evaluation</span></span>

<span data-ttu-id="99321-107">W poniższym przykładzie metoda pomocnicza służy do standaryzacji adresów URL dla blogów, które są zwracane z bazy danych SQL Server.</span><span class="sxs-lookup"><span data-stu-id="99321-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="99321-108">Ponieważ dostawca SQL Server nie ma wglądu w sposób implementacji tej metody, nie można go przetłumaczyć na SQL.</span><span class="sxs-lookup"><span data-stu-id="99321-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="99321-109">Wszystkie inne aspekty zapytania są oceniane w bazie danych, ale przekazywanie zwracanych `URL` przez tę metodę jest wykonywane na kliencie.</span><span class="sxs-lookup"><span data-stu-id="99321-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
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

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="99321-110">Problemy z wydajnością oceny klienta</span><span class="sxs-lookup"><span data-stu-id="99321-110">Client evaluation performance issues</span></span>

<span data-ttu-id="99321-111">Podczas gdy Ocena klienta może być bardzo przydatna, w niektórych przypadkach może to skutkować niską wydajnością.</span><span class="sxs-lookup"><span data-stu-id="99321-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="99321-112">Rozważmy następujące zapytanie, gdzie metoda pomocnika jest teraz używana w filtrze.</span><span class="sxs-lookup"><span data-stu-id="99321-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="99321-113">Ponieważ nie można wykonać tej operacji w bazie danych, wszystkie dane są pobierane do pamięci, a następnie na kliencie jest stosowany filtr.</span><span class="sxs-lookup"><span data-stu-id="99321-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="99321-114">W zależności od ilości danych i ilości danych, które są odfiltrowane, może to spowodować spadek wydajności.</span><span class="sxs-lookup"><span data-stu-id="99321-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="99321-115">Rejestrowanie oceny klienta</span><span class="sxs-lookup"><span data-stu-id="99321-115">Client evaluation logging</span></span>

<span data-ttu-id="99321-116">Domyślnie EF Core będzie rejestrował ostrzeżenie podczas przeprowadzania oceny klienta.</span><span class="sxs-lookup"><span data-stu-id="99321-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="99321-117">Zobacz [Rejestrowanie](../miscellaneous/logging.md) , aby uzyskać więcej informacji na temat wyświetlania danych wyjściowych rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="99321-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="99321-118">Zachowanie opcjonalne: Zgłoś wyjątek do oceny klienta</span><span class="sxs-lookup"><span data-stu-id="99321-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="99321-119">Można zmienić zachowanie podczas oceny klienta, aby zgłosić lub nic się nie dzieje.</span><span class="sxs-lookup"><span data-stu-id="99321-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="99321-120">Jest to wykonywane podczas konfigurowania opcji dla kontekstu — zwykle w programie `DbContext.OnConfiguring`, lub w `Startup.cs` przypadku korzystania z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99321-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
