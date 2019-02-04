---
title: Śledzenie programu vs. Zapytania śledzenia nie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 6c5d516fcb3950ae168860029660e1b1061546b8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668781"
---
# <a name="tracking-vs-no-tracking-queries"></a>Śledzenie programu vs. Bez śledzenia zapytań

Czy platformy Entity Framework Core przechowuje informacje o wystąpienie jednostki w jego śledzenie zmian do śledzenia zachowania formantów. Jeśli jednostka jest śledzona, wszelkie zmiany wykryte w jednostce zostaną utrwalone w bazie danych podczas `SaveChanges()`. Entity Framework Core będzie również konfigurowania właściwości nawigacji między jednostkami, które są uzyskiwane z zapytania śledzenia i jednostek, które wcześniej zostały załadowane do wystąpienia typu DbContext.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="tracking-queries"></a>Śledzenia zapytań

Domyślnie są śledzenia zapytań, które zwracają typów jednostek. Oznacza to, można wprowadzić zmiany do tych wystąpień jednostki i utrwalonych te zmiany przez `SaveChanges()`.

W poniższym przykładzie zostanie wykryte i utrwalone w bazie danych podczas zmiany klasyfikacji blogi `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Bez śledzenia zapytań

Nie śledzenia zapytań są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu. Są one szybsze wykonywanie, ponieważ nie ma potrzeby informacje o monitorowaniu zmian konfiguracji.

Można wymienić określone zapytanie do śledzenia nie można:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Możesz również zmienić domyślne zachowanie na poziomie wystąpienia kontekstu śledzenia:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Nie śledzenia zapytań w dalszym ciągu wykonywać rozwiązanie tożsamości w ramach wykonywania zapytania. Jeśli zestaw wyników zawiera tej samej jednostki wiele razy, to samo wystąpienie elementu Klasa jednostki zostanie zwrócony dla każdego wystąpienia w zestawie wyników. Jednak słabe odwołania są używane do śledzenia jednostek, które już zostały zwrócone. Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres, a następnie uruchamia wyrzucanie elementów bezużytecznych, może pojawić się nowe wystąpienie jednostki. Aby uzyskać więcej informacji, zobacz [jak działa zapytanie](overview.md).

## <a name="tracking-and-projections"></a>Śledzenie i projekcji

Nawet jeśli typ wyniku zapytania nie jest typem jednostki, jeśli wynik zawiera typy jednostek nadal będą one śledzone domyślnie. W następującym zapytaniu, która zwraca typ anonimowy, wystąpienia `Blog` w wyniku będą śledzone zestawu.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Jeśli zestaw wyników nie zawiera żadnych typów jednostek, Brak śledzenia jest wykonywana. W następującym zapytaniu, która zwraca typ anonimowy z niektórych wartości z jednostki (ale nie wystąpień typu rzeczywistego jednostek) nie jest Brak śledzenia wykonywane.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
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
