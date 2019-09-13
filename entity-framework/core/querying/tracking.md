---
title: Śledzenie a Zapytania bez śledzenia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921656"
---
# <a name="tracking-vs-no-tracking-queries"></a>Śledzenie a Zapytania bez śledzenia

Śledzenie zachowania kontroluje, czy Entity Framework Core będą utrzymywać informacje o wystąpieniu jednostki w jego monitorze zmian. Jeśli jednostka jest śledzona, wszelkie zmiany wykryte w jednostce zostaną utrwalone w bazie danych w trakcie `SaveChanges()`. Entity Framework Core również poprawi właściwości nawigacji między jednostkami uzyskanymi z zapytania śledzenia i jednostek, które zostały wcześniej załadowane do wystąpienia DbContext.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="tracking-queries"></a>Zapytania śledzenia

Domyślnie zapytania zwracające typy jednostek są śledzone. Oznacza to, że można wprowadzać zmiany w tych wystąpieniach jednostek i mieć te zmiany `SaveChanges()`utrwalane przez program.

W poniższym przykładzie zmiana klasyfikacji blogów zostanie wykryta i utrwalona w bazie danych w trakcie `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Zapytania bez śledzenia

Zapytania śledzenia nie są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu. Są one szybciej wykonywane, ponieważ nie ma potrzeby konfigurowania informacji o śledzeniu zmian.

Pojedyncze zapytanie można zamienić na brak śledzenia:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Możesz również zmienić domyślne zachowanie śledzenia na poziomie wystąpienia kontekstu:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Żadne zapytania śledzenia nadal nie wykonują rozpoznawania tożsamości w ramach wykonywania zapytania. Jeśli zestaw wyników zawiera tę samą jednostkę wiele razy, to samo wystąpienie klasy Entity zostanie zwrócone dla każdego wystąpienia w zestawie wyników. Jednak słabe odwołania są używane do śledzenia jednostek, które zostały już zwrócone. Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres i zostanie uruchomione odzyskiwanie pamięci, może zostać wyświetlone nowe wystąpienie jednostki. Aby uzyskać więcej informacji, zobacz [jak działa zapytanie](overview.md).

## <a name="tracking-and-projections"></a>Śledzenie i projekcje

Nawet jeśli typ wyniku zapytania nie jest typem jednostki, jeśli wynik zawiera typy jednostek, nadal będą domyślnie śledzone. W poniższym zapytaniu, które zwraca typ anonimowy, wystąpienia `Blog` w zestawie wyników będą śledzone.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Jeśli zestaw wyników nie zawiera żadnych typów jednostek, śledzenie nie jest wykonywane. W poniższym zapytaniu, które zwraca typ anonimowy z niektórymi wartościami z jednostki (ale nie wystąpienia rzeczywistego typu jednostki), nie wykonano śledzenia.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
