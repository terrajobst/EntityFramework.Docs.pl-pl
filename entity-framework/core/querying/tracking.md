---
title: Śledzenie a Zapytania bez śledzenia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 588dee012039ce5ecc83f0ecf263a4ea6ca38c29
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181984"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="fd3ca-102">Śledzenie a Zapytania bez śledzenia</span><span class="sxs-lookup"><span data-stu-id="fd3ca-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="fd3ca-103">Śledzenie zachowania kontroluje, czy Entity Framework Core będą utrzymywać informacje o wystąpieniu jednostki w jego monitorze zmian.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="fd3ca-104">W przypadku śledzenia jednostki wszystkie zmiany wykryte w jednostce zostaną utrwalone w bazie danych podczas `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="fd3ca-105">Entity Framework Core również poprawi właściwości nawigacji między jednostkami uzyskanymi z zapytania śledzenia i jednostek, które zostały wcześniej załadowane do wystąpienia DbContext.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="fd3ca-106">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="fd3ca-107">Zapytania śledzenia</span><span class="sxs-lookup"><span data-stu-id="fd3ca-107">Tracking queries</span></span>

<span data-ttu-id="fd3ca-108">Domyślnie zapytania zwracające typy jednostek są śledzone.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="fd3ca-109">Oznacza to, że można wprowadzać zmiany w tych wystąpieniach jednostek i mieć te zmiany utrwalane przez `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="fd3ca-110">W poniższym przykładzie zmiana klasyfikacji blogów zostanie wykryta i utrwalona w bazie danych podczas `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="fd3ca-111">Zapytania bez śledzenia</span><span class="sxs-lookup"><span data-stu-id="fd3ca-111">No-tracking queries</span></span>

<span data-ttu-id="fd3ca-112">Zapytania śledzenia nie są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="fd3ca-113">Są one szybciej wykonywane, ponieważ nie ma potrzeby konfigurowania informacji o śledzeniu zmian.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="fd3ca-114">Pojedyncze zapytanie można zamienić na brak śledzenia:</span><span class="sxs-lookup"><span data-stu-id="fd3ca-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="fd3ca-115">Możesz również zmienić domyślne zachowanie śledzenia na poziomie wystąpienia kontekstu:</span><span class="sxs-lookup"><span data-stu-id="fd3ca-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="fd3ca-116">Żadne zapytania śledzenia nadal nie wykonują rozpoznawania tożsamości w ramach wykonywania zapytania.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-116">No tracking queries still perform identity resolution within the executing query.</span></span> <span data-ttu-id="fd3ca-117">Jeśli zestaw wyników zawiera tę samą jednostkę wiele razy, to samo wystąpienie klasy Entity zostanie zwrócone dla każdego wystąpienia w zestawie wyników.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="fd3ca-118">Jednak słabe odwołania są używane do śledzenia jednostek, które zostały już zwrócone.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="fd3ca-119">Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres i zostanie uruchomione odzyskiwanie pamięci, może zostać wyświetlone nowe wystąpienie jednostki.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="fd3ca-120">Aby uzyskać więcej informacji, zobacz [jak działa zapytanie](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="fd3ca-120">For more information, see [How Query Works](xref:core/querying/how-query-works).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="fd3ca-121">Śledzenie i projekcje</span><span class="sxs-lookup"><span data-stu-id="fd3ca-121">Tracking and projections</span></span>

<span data-ttu-id="fd3ca-122">Nawet jeśli typ wyniku zapytania nie jest typem jednostki, jeśli wynik zawiera typy jednostek, nadal będą domyślnie śledzone.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="fd3ca-123">W poniższym zapytaniu, które zwraca typ anonimowy, będą śledzone wystąpienia `Blog` w zestawie wyników.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="fd3ca-124">Jeśli zestaw wyników nie zawiera żadnych typów jednostek, śledzenie nie jest wykonywane.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="fd3ca-125">W poniższym zapytaniu, które zwraca typ anonimowy z niektórymi wartościami z jednostki (ale nie wystąpienia rzeczywistego typu jednostki), nie wykonano śledzenia.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
