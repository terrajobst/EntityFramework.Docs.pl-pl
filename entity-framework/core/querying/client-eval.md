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
# <a name="client-vs-server-evaluation"></a>Klient a Ocena serwera

Entity Framework Core obsługuje części kwerendy ocenianej na kliencie i części, które są przekazywane do bazy danych. Aby określić, które części zapytania będą oceniane w bazie danych, należy do dostawcy bazy danych.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="client-evaluation"></a>Ocena klienta

W poniższym przykładzie metoda pomocnicza służy do standaryzacji adresów URL dla blogów, które są zwracane z bazy danych SQL Server. Ponieważ dostawca SQL Server nie ma wglądu w sposób implementacji tej metody, nie można go przetłumaczyć na SQL. Wszystkie inne aspekty zapytania są oceniane w bazie danych, ale przekazywanie zwracanych `URL` przez tę metodę jest wykonywane na kliencie.

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

## <a name="client-evaluation-performance-issues"></a>Problemy z wydajnością oceny klienta

Podczas gdy Ocena klienta może być bardzo przydatna, w niektórych przypadkach może to skutkować niską wydajnością. Rozważmy następujące zapytanie, gdzie metoda pomocnika jest teraz używana w filtrze. Ponieważ nie można wykonać tej operacji w bazie danych, wszystkie dane są pobierane do pamięci, a następnie na kliencie jest stosowany filtr. W zależności od ilości danych i ilości danych, które są odfiltrowane, może to spowodować spadek wydajności.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Rejestrowanie oceny klienta

Domyślnie EF Core będzie rejestrował ostrzeżenie podczas przeprowadzania oceny klienta. Zobacz [Rejestrowanie](../miscellaneous/logging.md) , aby uzyskać więcej informacji na temat wyświetlania danych wyjściowych rejestrowania. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Zachowanie opcjonalne: Zgłoś wyjątek do oceny klienta

Można zmienić zachowanie podczas oceny klienta, aby zgłosić lub nic się nie dzieje. Jest to wykonywane podczas konfigurowania opcji dla kontekstu — zwykle w programie `DbContext.OnConfiguring`, lub w `Startup.cs` przypadku korzystania z ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
