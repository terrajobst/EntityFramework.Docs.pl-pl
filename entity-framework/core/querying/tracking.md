---
title: Śledzenie programu vs. Zapytania śledzenia nie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 985adc795f379199a3bacc985843f32f3168cf64
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993358"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="9df2c-102">Śledzenie programu vs. Bez śledzenia zapytań</span><span class="sxs-lookup"><span data-stu-id="9df2c-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="9df2c-103">Czy platformy Entity Framework Core przechowuje informacje o wystąpienie jednostki w jego śledzenie zmian do śledzenia zachowania formantów.</span><span class="sxs-lookup"><span data-stu-id="9df2c-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="9df2c-104">Jeśli jednostka jest śledzona, wszelkie zmiany wykryte w jednostce zostaną utrwalone w bazie danych podczas `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9df2c-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="9df2c-105">Entity Framework Core będzie również konfigurowania właściwości nawigacji między jednostkami, które są uzyskiwane z zapytania śledzenia i jednostek, które wcześniej zostały załadowane do wystąpienia typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="9df2c-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="9df2c-106">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="9df2c-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="9df2c-107">Śledzenia zapytań</span><span class="sxs-lookup"><span data-stu-id="9df2c-107">Tracking queries</span></span>

<span data-ttu-id="9df2c-108">Domyślnie są śledzenia zapytań, które zwracają typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="9df2c-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="9df2c-109">Oznacza to, można wprowadzić zmiany do tych wystąpień jednostki i utrwalonych te zmiany przez `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9df2c-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="9df2c-110">W poniższym przykładzie zostanie wykryte i utrwalone w bazie danych podczas zmiany klasyfikacji blogi `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="9df2c-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="9df2c-111">Bez śledzenia zapytań</span><span class="sxs-lookup"><span data-stu-id="9df2c-111">No-tracking queries</span></span>

<span data-ttu-id="9df2c-112">Nie śledzenia zapytań są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="9df2c-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="9df2c-113">Są one szybsze wykonywanie, ponieważ nie ma potrzeby informacje o monitorowaniu zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9df2c-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="9df2c-114">Można wymienić określone zapytanie do śledzenia nie można:</span><span class="sxs-lookup"><span data-stu-id="9df2c-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="9df2c-115">Możesz również zmienić domyślne zachowanie na poziomie wystąpienia kontekstu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="9df2c-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="9df2c-116">Nie śledzenia zapytań w dalszym ciągu wykonywać rozwiązanie tożsamości w ramach zapytania przesłać.</span><span class="sxs-lookup"><span data-stu-id="9df2c-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="9df2c-117">Jeśli zestaw wyników zawiera tej samej jednostki wiele razy, to samo wystąpienie elementu Klasa jednostki zostanie zwrócony dla każdego wystąpienia w zestawie wyników.</span><span class="sxs-lookup"><span data-stu-id="9df2c-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="9df2c-118">Jednak słabe odwołania są używane do śledzenia jednostek, które już zostały zwrócone.</span><span class="sxs-lookup"><span data-stu-id="9df2c-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="9df2c-119">Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres, a następnie uruchamia wyrzucanie elementów bezużytecznych, może pojawić się nowe wystąpienie jednostki.</span><span class="sxs-lookup"><span data-stu-id="9df2c-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="9df2c-120">Aby uzyskać więcej informacji, zobacz [jak działa zapytanie](overview.md).</span><span class="sxs-lookup"><span data-stu-id="9df2c-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="9df2c-121">Śledzenie i projekcji</span><span class="sxs-lookup"><span data-stu-id="9df2c-121">Tracking and projections</span></span>

<span data-ttu-id="9df2c-122">Nawet jeśli typ wyniku zapytania nie jest typem jednostki, jeśli wynik zawiera typy jednostek nadal będą one śledzone domyślnie.</span><span class="sxs-lookup"><span data-stu-id="9df2c-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="9df2c-123">W następującym zapytaniu, która zwraca typ anonimowy, wystąpienia `Blog` w wyniku będą śledzone zestawu.</span><span class="sxs-lookup"><span data-stu-id="9df2c-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="9df2c-124">Jeśli zestaw wyników nie zawiera żadnych typów jednostek, Brak śledzenia jest wykonywana.</span><span class="sxs-lookup"><span data-stu-id="9df2c-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="9df2c-125">W następującym zapytaniu, która zwraca typ anonimowy z niektórych wartości z jednostki (ale nie wystąpień typu rzeczywistego jednostek) nie jest Brak śledzenia wykonywane.</span><span class="sxs-lookup"><span data-stu-id="9df2c-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
