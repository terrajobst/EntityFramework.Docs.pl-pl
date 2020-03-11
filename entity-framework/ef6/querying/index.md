---
title: Wykonywanie zapytań i znajdowanie jednostek — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417169"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="60098-102">Wykonywanie zapytań i znajdowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="60098-102">Querying and Finding Entities</span></span>
<span data-ttu-id="60098-103">W tym temacie omówiono różne sposoby wykonywania zapytań dotyczących danych przy użyciu Entity Framework, w tym LINQ i metody Find.</span><span class="sxs-lookup"><span data-stu-id="60098-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="60098-104">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="60098-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="60098-105">Znajdowanie jednostek przy użyciu zapytania</span><span class="sxs-lookup"><span data-stu-id="60098-105">Finding entities using a query</span></span>  

<span data-ttu-id="60098-106">Nieogólnymi i IDbSet implementują interfejs IQueryable i dlatego mogą być używane jako punkt wyjścia do pisania zapytania LINQ względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60098-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="60098-107">To nie jest odpowiednie miejsce do szczegółowego omówienia LINQ, ale poniżej przedstawiono kilka prostych przykładów:</span><span class="sxs-lookup"><span data-stu-id="60098-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

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

<span data-ttu-id="60098-108">Należy pamiętać, że Nieogólnymi i IDbSet zawsze tworzą zapytania względem bazy danych i zawsze będą obejmowały rundę do bazy danych, nawet jeśli zwrócona jednostka już istnieje w kontekście.</span><span class="sxs-lookup"><span data-stu-id="60098-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="60098-109">Zapytanie jest wykonywane w odniesieniu do bazy danych, gdy:</span><span class="sxs-lookup"><span data-stu-id="60098-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="60098-110">Jest on wyliczany przez instrukcję **foreach** (C#) lub **dla każdej** instrukcji (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="60098-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="60098-111">Jest on wyliczany przez operację kolekcji, taką jak [ToArray —](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)lub [ToList —](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="60098-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="60098-112">Operatory LINQ, takie jak [First](https://msdn.microsoft.com/library/bb291976) lub [any](https://msdn.microsoft.com/library/bb337697) , są określone w najbardziej zewnętrznej części zapytania.</span><span class="sxs-lookup"><span data-stu-id="60098-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="60098-113">Następujące metody są wywoływane: Metoda [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) Extension w Nieogólnymi, [DbEntityEntry. reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)i Database. ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="60098-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="60098-114">Gdy wyniki są zwracane z bazy danych, obiekty, które nie istnieją w kontekście, są dołączane do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="60098-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="60098-115">Jeśli obiekt znajduje się już w kontekście, zwracany jest istniejący obiekt (bieżące i oryginalne wartości właściwości obiektu w tym wpisie **nie** są zastępowane wartościami bazy danych).</span><span class="sxs-lookup"><span data-stu-id="60098-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="60098-116">Podczas wykonywania zapytania jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych, nie są zwracane jako część zestawu wyników.</span><span class="sxs-lookup"><span data-stu-id="60098-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="60098-117">Aby uzyskać dane, które znajdują się w kontekście, zobacz [dane lokalne](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="60098-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="60098-118">Jeśli zapytanie nie zwraca żadnych wierszy z bazy danych, wynik będzie pustą kolekcją, a nie **wartością null**.</span><span class="sxs-lookup"><span data-stu-id="60098-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="60098-119">Znajdowanie jednostek przy użyciu kluczy podstawowych</span><span class="sxs-lookup"><span data-stu-id="60098-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="60098-120">Metoda Find w Nieogólnymi używa wartości klucza podstawowego, aby próbować znaleźć jednostkę śledzoną przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="60098-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="60098-121">Jeśli jednostka nie zostanie znaleziona w kontekście, zapytanie zostanie wysłane do bazy danych, aby znaleźć w niej jednostkę.</span><span class="sxs-lookup"><span data-stu-id="60098-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="60098-122">Zwracana jest wartość null, jeśli jednostka nie została znaleziona w kontekście lub w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="60098-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="60098-123">Wyszukiwanie różni się od użycia zapytania na dwa znaczące sposoby:</span><span class="sxs-lookup"><span data-stu-id="60098-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="60098-124">Przepodróżowanie do bazy danych zostanie wykonane tylko wtedy, gdy jednostka z danym kluczem nie zostanie znaleziona w kontekście.</span><span class="sxs-lookup"><span data-stu-id="60098-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="60098-125">Znajdź zwróci jednostki, które znajdują się w stanie dodany.</span><span class="sxs-lookup"><span data-stu-id="60098-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="60098-126">Oznacza to, że Znajdź zwróci jednostki, które zostały dodane do kontekstu, ale nie zostały jeszcze zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="60098-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="60098-127">Znajdowanie jednostki według klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="60098-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="60098-128">Poniższy kod przedstawia niektóre zastosowania wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="60098-128">The following code shows some uses of Find:</span></span>  

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

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="60098-129">Znajdowanie jednostki według złożonego klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="60098-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="60098-130">Entity Framework umożliwia jednostkom posiadanie kluczy złożonych — to jest klucz składający się z więcej niż jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="60098-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="60098-131">Można na przykład mieć jednostkę BlogSettings, która reprezentuje ustawienia użytkowników dla określonego bloga.</span><span class="sxs-lookup"><span data-stu-id="60098-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="60098-132">Ze względu na to, że użytkownik będzie miał tylko jeden BlogSettings dla każdego blogu, który można wybrać, aby klucz podstawowy BlogSettings kombinację BlogId i nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="60098-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="60098-133">Poniższy kod próbuje znaleźć BlogSettings z BlogId = 3 i username = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="60098-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="60098-134">Należy pamiętać, że jeśli masz klucze złożone, musisz użyć elementu ColumnAttribute lub interfejsu API Fluent, aby określić kolejność właściwości klucza złożonego.</span><span class="sxs-lookup"><span data-stu-id="60098-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="60098-135">Wywołanie metody Find musi używać tej kolejności podczas określania wartości, które tworzą klucz.</span><span class="sxs-lookup"><span data-stu-id="60098-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
