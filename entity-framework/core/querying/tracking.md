---
title: "Śledzenie programu vs. Zapytania śledzenia nie - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Śledzenie programu vs. Zapytania dotyczące śledzenia nie

Czy program Entity Framework Core przechowuje informacje o wystąpienia jednostek w jego śledzący zmiany do śledzenia kontroli zachowania. Jeśli śledzenia jednostki wykryto w jednostce wszelkie zmiany zostaną utrwalone w bazie danych podczas `SaveChanges()`. Entity Framework Core będzie również konfigurowania właściwości nawigacji między jednostek, które są uzyskiwane z zapytania śledzenia i jednostkami, które wcześniej zostały załadowane do wystąpienia typu DbContext.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="tracking-queries"></a>Zapytania dotyczące śledzenia

Domyślnie są śledzenia zapytań, które zwracają typów jednostek. Oznacza to, można wprowadzić zmiany do tych jednostek wystąpień i te zmiany utrwalonych przez `SaveChanges()`.

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

## <a name="no-tracking-queries"></a>Zapytania dotyczące śledzenia nie

Nie ma zapytań, śledzenia są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu. Są one szybsze można wykonać operacji, ponieważ nie istnieje potrzeba aby informacje o monitorowaniu zmian konfiguracji.

Można wymienić poszczególnych zapytań do śledzenia nie można:

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
> Nie ma zapytań, śledzenie nadal rozpoznawać tożsamości w ramach zapytania przesłać. Jeśli zestaw wyników zawiera tej samej jednostki wiele razy, zostanie zwrócony tego samego wystąpienia klasy jednostki dla każdego wystąpienia w zestawie wyników. Jednak słabe odwołania są używane do śledzenia jednostek, które już zostały zwrócone. Jeśli poprzedni wynik o tej samej tożsamości wykracza poza zakres i wyrzucanie elementów bezużytecznych działa, możesz uzyskać nowego wystąpienia jednostki. Aby uzyskać więcej informacji, zobacz [jak działa zapytanie](overview.md).

## <a name="tracking-and-projections"></a>Śledzenie i projekcji

Nawet jeśli typu wyników zapytania nie jest typem jednostki, jeśli wynik zawiera typy jednostek one nadal będą śledzone przez domyślne. W następującej kwerendy, która zwraca typu anonimowego wystąpienia `Blog` w wyniku będą śledzone zestawu.

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

Jeśli zestaw wyników nie zawiera żadnych typów jednostek, przeprowadzane jest Brak śledzenia. W następującej kwerendy, który zwraca typ anonimowy w przypadku niektórych wartości z jednostki (ale nie wystąpień rzeczywistego typu jednostki) nie należy nie śledzenia wykonywane.

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
