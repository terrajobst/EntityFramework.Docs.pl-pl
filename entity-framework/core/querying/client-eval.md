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

## <a name="disabling-client-evaluation"></a>Wyłączanie oceny klienta

Oceny klienta mogą być bardzo przydatne, w niektórych przypadkach go może spowodować obniżenie wydajności. Należy wziąć pod uwagę następujące zapytanie, której metody pomocnika teraz jest używane w filtrze. Ponieważ to nie może zostać wykonana w bazie danych, wszystkie dane są pobierane do pamięci, a następnie filtr jest stosowany na komputerze klienckim. W zależności od ilości danych, a ilość tych danych jest odfiltrowana może to spowodować spadek wydajności.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Domyślnie program EF Core będzie rejestrować ostrzeżenie podczas oceny klienta jest wykonywane. Zobacz [rejestrowania](../miscellaneous/logging.md) więcej informacji na temat wyświetlania danych wyjściowych rejestrowania. Można zmienić zachowanie w przypadku oceny klienta throw lub nic nie rób. Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` korzystania z platformy ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
