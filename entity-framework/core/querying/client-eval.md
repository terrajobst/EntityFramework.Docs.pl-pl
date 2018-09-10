---
title: Klient programu vs. Ocena serwera — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 47e22be274d02b5221c638d07151d9607aa7e24f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250806"
---
# <a name="client-vs-server-evaluation"></a>Klient programu vs. Ocena serwera

Entity Framework Core obsługuje części zapytania Trwa obliczanie na urządzeniu klienckim i części wypychania do bazy danych. Jest dostawca bazy danych, aby określić części zapytania, które zostanie obliczone w bazie danych.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="client-evaluation"></a>Oceny klienta

W poniższym przykładzie metoda pomocnika służy do standaryzacji adresy URL dla blogów, z którego są zwracane z bazy danych programu SQL Server. Ponieważ dostawca programu SQL Server nie wgląd w jaki sposób ta metoda jest implementowana, nie jest możliwe tłumaczenie SQL. Wszystkie inne aspekty zapytania są oceniane w bazie danych, ale przekazywanie zwracanego `URL` za pośrednictwem tej metody jest wykonywane na komputerze klienckim.

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

## <a name="client-evaluation-performance-issues"></a>Problemy z wydajnością oceny klienta

Oceny klienta mogą być bardzo przydatne, w niektórych przypadkach go może spowodować obniżenie wydajności. Należy wziąć pod uwagę następujące zapytanie, której metody pomocnika teraz jest używane w filtrze. Ponieważ to nie może zostać wykonana w bazie danych, wszystkie dane są pobierane do pamięci, a następnie filtr jest stosowany na komputerze klienckim. W zależności od ilości danych, a ilość tych danych jest odfiltrowana może to spowodować spadek wydajności.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Rejestrowanie oceny klienta

Domyślnie program EF Core będzie rejestrować ostrzeżenie podczas oceny klienta jest wykonywane. Zobacz [rejestrowania](../miscellaneous/logging.md) więcej informacji na temat wyświetlania danych wyjściowych rejestrowania. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Zachowanie opcjonalne: zgłoszenie wyjątku dla oceny klienta

Można zmienić zachowanie w przypadku oceny klienta throw lub nic nie rób. Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` korzystania z platformy ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
