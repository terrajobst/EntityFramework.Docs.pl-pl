---
title: Wykonywanie zapytań i wyszukiwanie jednostek - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
caps.latest.revision: 3
ms.openlocfilehash: 92467e1a93f576eca627cf7b7d2351054a882c2c
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067550"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="d4d5d-102">Wykonywanie zapytań i wyszukiwanie jednostek</span><span class="sxs-lookup"><span data-stu-id="d4d5d-102">Querying and Finding Entities</span></span>
<span data-ttu-id="d4d5d-103">W tym temacie omówiono różne sposoby, które można wyszukiwać dane przy użyciu platformy Entity Framework, w tym LINQ i metody Find.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="d4d5d-104">Techniki przedstawione w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i projektancie platformy EF.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="d4d5d-105">Wyszukiwanie jednostek przy użyciu zapytania</span><span class="sxs-lookup"><span data-stu-id="d4d5d-105">Finding entities using a query</span></span>  

<span data-ttu-id="d4d5d-106">DbSet IDbSet implementować interfejs IQueryable i dlatego może służyć jako punktu wyjścia do pisania zapytania LINQ w odniesieniu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="d4d5d-107">To nie jest właściwym miejscem dla szczegółowe omówienie LINQ, ale poniżej przedstawiono dwa proste przykłady:</span><span class="sxs-lookup"><span data-stu-id="d4d5d-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="d4d5d-108">Należy pamiętać, że DbSet IDbSet zawsze tworzenie zapytań względem bazy danych i zawsze będzie obejmować komunikacji dwustronnej w bazie danych, nawet jeśli zwróconych już istnieje w kontekście.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="d4d5d-109">Zapytanie jest wykonywane względem bazy danych po:</span><span class="sxs-lookup"><span data-stu-id="d4d5d-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="d4d5d-110">Są wyliczane przez **foreach** (C#) lub **dla każdego** — instrukcja (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="d4d5d-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="d4d5d-111">Jego są wyliczane przez operację kolekcji takich jak [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), lub [tolist —](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="d4d5d-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="d4d5d-112">Operatory LINQ, takich jak [pierwszy](https://msdn.microsoft.com/library/bb291976) lub [wszelkie](https://msdn.microsoft.com/library/bb337697) są określone w najbardziej zewnętrznej część zapytania.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="d4d5d-113">Następujące metody są wywoływane: [obciążenia](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) metody rozszerzenia na DbSet [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)i Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="d4d5d-114">Gdy wyniki są zwracane z bazy danych, obiekty, które nie istnieją w kontekście są dołączone do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="d4d5d-115">Jeśli obiekt jest już w kontekście, istniejący obiekt jest zwracany (są bieżące i oryginalne wartości właściwości obiektu we wpisie **nie** zastąpione wartościami bazy danych).</span><span class="sxs-lookup"><span data-stu-id="d4d5d-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="d4d5d-116">Podczas wykonywania zapytania jednostek, które zostały dodane do kontekstu, ale nie zostały zapisane w bazie danych nie są zwracane jako część zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="d4d5d-117">Aby uzyskać dane, które znajduje się w kontekście, zobacz [dane lokalne](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="d4d5d-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="d4d5d-118">Jeśli zapytanie zwraca żadnych wierszy z bazy danych, wynik będzie pustą kolekcję, zamiast **null**.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="d4d5d-119">Wyszukiwanie jednostek przy użyciu kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="d4d5d-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="d4d5d-120">Metody Find na DbSet używa wartość klucza podstawowego do podejmą próbę odnalezienia śledzone przez kontekst jednostki.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="d4d5d-121">Jeśli jednostka nie zostanie znaleziony w kontekście następnie kwerendę otrzymasz do bazy danych można znaleźć jednostki istnieje.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="d4d5d-122">Jeśli jednostka nie zostanie odnaleziona w kontekście lub w bazie danych, zwracana jest wartość null.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="d4d5d-123">Znajdź różni się od przy użyciu zapytania na dwa istotne sposoby:</span><span class="sxs-lookup"><span data-stu-id="d4d5d-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="d4d5d-124">Przesłania danych do bazy danych będą dostępne tylko w przypadku, jeśli jednostki z danym kluczem nie zostanie znaleziony w kontekście.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="d4d5d-125">Znajdź zwróci jednostek, które są w stanie dodany.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="d4d5d-126">Znajdź zwróci jednostek, które zostały dodane do kontekstu, ale nie zostały zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="d4d5d-127">Wyszukiwanie jednostek według klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="d4d5d-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="d4d5d-128">Poniższy kod pokazuje do niektórych zastosowań wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="d4d5d-128">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="d4d5d-129">Wyszukiwanie jednostki według złożony klucz podstawowy</span><span class="sxs-lookup"><span data-stu-id="d4d5d-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="d4d5d-130">Entity Framework umożliwia jednostki mają klucze złożone — czyli klucz, który składa się z więcej niż jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="d4d5d-131">Na przykład można mieć jednostki BlogSettings, która reprezentuje ustawienia użytkowników, dla konkretnego blogu.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="d4d5d-132">Ponieważ użytkownik będzie istniało tylko jeden BlogSettings dla każdego bloga, które można wykonać następujące akcje wybrała się klucz podstawowy BlogSettings kombinacją BlogId i nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="d4d5d-133">Poniższy kod próbuje znaleźć BlogSettings z BlogId = 3, a nazwa użytkownika = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="d4d5d-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="d4d5d-134">Należy pamiętać, że w przypadku kluczy złożonych należy użyć ColumnAttribute lub interfejsu API fluent Aby określić kolejność właściwości klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="d4d5d-135">Podczas określania wartości, które tworzą klucz, wywołania do znalezienia należy użyć tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="d4d5d-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
