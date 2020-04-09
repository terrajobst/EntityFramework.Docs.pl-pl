---
title: Wykonywanie zapytań i znajdowanie jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417169"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="0677e-102">Wykonywanie zapytań i znajdowanie encji</span><span class="sxs-lookup"><span data-stu-id="0677e-102">Querying and Finding Entities</span></span>
<span data-ttu-id="0677e-103">W tym temacie opisano różne sposoby wykonywania zapytań o dane przy użyciu entity framework, w tym LINQ i Find metody.</span><span class="sxs-lookup"><span data-stu-id="0677e-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="0677e-104">Techniki pokazane w tym temacie stosuje się jednakowo do modeli utworzonych za pomocą Code First i EF Designer.</span><span class="sxs-lookup"><span data-stu-id="0677e-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="0677e-105">Znajdowanie encji przy użyciu kwerendy</span><span class="sxs-lookup"><span data-stu-id="0677e-105">Finding entities using a query</span></span>  

<span data-ttu-id="0677e-106">DbSet i IDbSet implementują IQueryable i dlatego mogą być używane jako punkt wyjścia do pisania zapytania LINQ w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0677e-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="0677e-107">Nie jest to odpowiednie miejsce do dogłębnej dyskusji linq, ale oto kilka prostych przykładów:</span><span class="sxs-lookup"><span data-stu-id="0677e-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

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

<span data-ttu-id="0677e-108">Należy zauważyć, że DbSet i IDbSet zawsze tworzyć zapytania względem bazy danych i zawsze będzie obejmować podróż w obie strony do bazy danych, nawet jeśli zwrócone jednostki już istnieją w kontekście.</span><span class="sxs-lookup"><span data-stu-id="0677e-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="0677e-109">Kwerenda jest wykonywana względem bazy danych, gdy:</span><span class="sxs-lookup"><span data-stu-id="0677e-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="0677e-110">Jest wyliczona przez **foreach** (C#) lub **for Each** (Visual Basic) instrukcji.</span><span class="sxs-lookup"><span data-stu-id="0677e-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="0677e-111">Jest wyliczona przez operację zbierania, taką jak [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)lub [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="0677e-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="0677e-112">Linq operatorów, takich jak [First](https://msdn.microsoft.com/library/bb291976) lub [Any](https://msdn.microsoft.com/library/bb337697) są określone w najbardziej zewnętrznej części kwerendy.</span><span class="sxs-lookup"><span data-stu-id="0677e-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="0677e-113">Następujące metody są wywoływane: [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)i Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="0677e-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="0677e-114">Gdy wyniki są zwracane z bazy danych, obiekty, które nie istnieją w kontekście są dołączone do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="0677e-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="0677e-115">Jeśli obiekt jest już w kontekście, zwracany jest istniejący obiekt (bieżące i oryginalne wartości właściwości obiektu we wpisie **nie** są zastępowane wartościami bazy danych).</span><span class="sxs-lookup"><span data-stu-id="0677e-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="0677e-116">Podczas wykonywania kwerendy jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych, nie są zwracane jako część zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="0677e-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="0677e-117">Aby uzyskać dane, które są w kontekście, zobacz [Dane lokalne](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="0677e-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="0677e-118">Jeśli kwerenda zwraca żadnych wierszy z bazy danych, wynik będzie pustą kolekcją, a nie **null**.</span><span class="sxs-lookup"><span data-stu-id="0677e-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="0677e-119">Znajdowanie encji przy użyciu kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="0677e-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="0677e-120">Find Metoda na DbSet używa wartości klucza podstawowego, aby spróbować znaleźć jednostkę śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="0677e-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="0677e-121">Jeśli jednostka nie zostanie znaleziona w kontekście, kwerenda zostanie wysłana do bazy danych w celu znalezienia tej jednostki.</span><span class="sxs-lookup"><span data-stu-id="0677e-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="0677e-122">Null jest zwracany, jeśli jednostka nie zostanie znaleziona w kontekście lub w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0677e-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="0677e-123">Znajdź różni się od używania kwerendy na dwa istotne sposoby:</span><span class="sxs-lookup"><span data-stu-id="0677e-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="0677e-124">Podróż w obie strony do bazy danych zostanie dokonana tylko wtedy, gdy jednostka z danym kluczem nie zostanie znaleziona w kontekście.</span><span class="sxs-lookup"><span data-stu-id="0677e-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="0677e-125">Znajdź zwróci jednostki, które są w stanie Dodane.</span><span class="sxs-lookup"><span data-stu-id="0677e-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="0677e-126">Oznacza to, że Find zwróci jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0677e-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="0677e-127">Znajdowanie jednostki według klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="0677e-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="0677e-128">Poniższy kod pokazuje niektóre zastosowania funkcji Znajdź:</span><span class="sxs-lookup"><span data-stu-id="0677e-128">The following code shows some uses of Find:</span></span>  

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

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="0677e-129">Znajdowanie jednostki według złożonego klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="0677e-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="0677e-130">Entity Framework umożliwia jednostek mają klucze złożone — to klucz, który składa się z więcej niż jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="0677e-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="0677e-131">Na przykład można mieć BlogSettings jednostki, która reprezentuje ustawienia użytkowników dla określonego bloga.</span><span class="sxs-lookup"><span data-stu-id="0677e-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="0677e-132">Ponieważ użytkownik będzie tylko kiedykolwiek jeden BlogSettings dla każdego bloga można wybrać, aby podstawowy klucz BlogSettings połączenie BlogId i nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0677e-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="0677e-133">Następujący kod próbuje znaleźć BlogSettings z BlogId = 3 i Nazwa użytkownika = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="0677e-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="0677e-134">Należy zauważyć, że gdy masz klucze złożone, należy użyć ColumnAttribute lub płynnego interfejsu API, aby określić kolejność właściwości klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="0677e-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="0677e-135">Wywołanie Find musi używać tej kolejności podczas określania wartości, które tworzą klucz.</span><span class="sxs-lookup"><span data-stu-id="0677e-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
