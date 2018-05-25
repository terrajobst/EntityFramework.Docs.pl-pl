---
title: Śledzenie programu vs. Zapytania śledzenia nie - EF Core
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
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="96d49-102">Śledzenie programu vs. Zapytania dotyczące śledzenia nie</span><span class="sxs-lookup"><span data-stu-id="96d49-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="96d49-103">Czy program Entity Framework Core przechowuje informacje o wystąpienia jednostek w jego śledzący zmiany do śledzenia kontroli zachowania.</span><span class="sxs-lookup"><span data-stu-id="96d49-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="96d49-104">Jeśli śledzenia jednostki wykryto w jednostce wszelkie zmiany zostaną utrwalone w bazie danych podczas `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="96d49-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="96d49-105">Entity Framework Core będzie również konfigurowania właściwości nawigacji między jednostek, które są uzyskiwane z zapytania śledzenia i jednostkami, które wcześniej zostały załadowane do wystąpienia typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="96d49-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="96d49-106">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="96d49-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="96d49-107">Zapytania dotyczące śledzenia</span><span class="sxs-lookup"><span data-stu-id="96d49-107">Tracking queries</span></span>

<span data-ttu-id="96d49-108">Domyślnie są śledzenia zapytań, które zwracają typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="96d49-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="96d49-109">Oznacza to, można wprowadzić zmiany do tych jednostek wystąpień i te zmiany utrwalonych przez `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="96d49-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="96d49-110">W poniższym przykładzie zostanie wykryte i utrwalone w bazie danych podczas zmiany klasyfikacji blogi `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="96d49-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="96d49-111">Zapytania dotyczące śledzenia nie</span><span class="sxs-lookup"><span data-stu-id="96d49-111">No-tracking queries</span></span>

<span data-ttu-id="96d49-112">Nie ma zapytań, śledzenia są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="96d49-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="96d49-113">Są one szybsze można wykonać operacji, ponieważ nie istnieje potrzeba aby informacje o monitorowaniu zmian konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="96d49-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="96d49-114">Można wymienić poszczególnych zapytań do śledzenia nie można:</span><span class="sxs-lookup"><span data-stu-id="96d49-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="96d49-115">Możesz również zmienić domyślne zachowanie na poziomie wystąpienia kontekstu śledzenia:</span><span class="sxs-lookup"><span data-stu-id="96d49-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="96d49-116">Nie ma zapytań, śledzenie nadal rozpoznawać tożsamości w ramach zapytania przesłać.</span><span class="sxs-lookup"><span data-stu-id="96d49-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="96d49-117">Jeśli zestaw wyników zawiera tej samej jednostki wiele razy, zostanie zwrócony tego samego wystąpienia klasy jednostki dla każdego wystąpienia w zestawie wyników.</span><span class="sxs-lookup"><span data-stu-id="96d49-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="96d49-118">Jednak słabe odwołania są używane do śledzenia jednostek, które już zostały zwrócone.</span><span class="sxs-lookup"><span data-stu-id="96d49-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="96d49-119">Jeśli poprzedni wynik o tej samej tożsamości wykracza poza zakres i wyrzucanie elementów bezużytecznych działa, możesz uzyskać nowego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="96d49-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="96d49-120">Aby uzyskać więcej informacji, zobacz [jak działa zapytanie](overview.md).</span><span class="sxs-lookup"><span data-stu-id="96d49-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="96d49-121">Śledzenie i projekcji</span><span class="sxs-lookup"><span data-stu-id="96d49-121">Tracking and projections</span></span>

<span data-ttu-id="96d49-122">Nawet jeśli typu wyników zapytania nie jest typem jednostki, jeśli wynik zawiera typy jednostek one nadal będą śledzone przez domyślne.</span><span class="sxs-lookup"><span data-stu-id="96d49-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="96d49-123">W następującej kwerendy, która zwraca typu anonimowego wystąpienia `Blog` w wyniku będą śledzone zestawu.</span><span class="sxs-lookup"><span data-stu-id="96d49-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="96d49-124">Jeśli zestaw wyników nie zawiera żadnych typów jednostek, przeprowadzane jest Brak śledzenia.</span><span class="sxs-lookup"><span data-stu-id="96d49-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="96d49-125">W następującej kwerendy, który zwraca typ anonimowy w przypadku niektórych wartości z jednostki (ale nie wystąpień rzeczywistego typu jednostki) nie należy nie śledzenia wykonywane.</span><span class="sxs-lookup"><span data-stu-id="96d49-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
