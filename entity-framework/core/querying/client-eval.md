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
# <a name="client-vs-server-evaluation"></a>Klient programu vs. Ocena serwera

Program Entity Framework Core obsługuje części zapytania są oceniane na kliencie i części przekazywanej do bazy danych. Jest dostawca bazy danych, aby określić części zapytania, które zostanie obliczone w bazie danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="client-evaluation"></a>Ocena klienta

W poniższym przykładzie metoda pomocnika służy do normalizacji adresów URL dla blogów, z którego są zwracane z bazy danych programu SQL Server. Ponieważ dostawca programu SQL Server nie ma wgląd implementowania tej metody, nie jest możliwe umożliwiło SQL. Innych aspektów zapytania są oceniane w bazie danych, ale przekazywanie zwróconego `URL` za pomocą tej metody jest wykonywana na kliencie.

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

Podczas oceny klienta może być bardzo przydatne, w niektórych przypadkach, które mogą skutkować niską wydajnością. Należy wziąć pod uwagę następujące zapytanie, gdy metoda pomocnika teraz jest używana w filtrze. Ponieważ to nie można wykonać w bazie danych, wszystkie dane są pobierane do pamięci, a następnie filtr jest stosowany na kliencie. W zależności od ilości danych i ilości danych jest odfiltrowany może to spowodować niską wydajnością.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Domyślnie EF Core będzie rejestrować ostrzeżenie podczas oceny klienta jest wykonywana. Zobacz [rejestrowanie](../miscellaneous/logging.md) Aby uzyskać więcej informacji o wyświetlaniu dane wyjściowe rejestrowania. Zachowanie można zmienić podczas oceny klienta throw lub nie rób. Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` Jeśli używasz platformy ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
