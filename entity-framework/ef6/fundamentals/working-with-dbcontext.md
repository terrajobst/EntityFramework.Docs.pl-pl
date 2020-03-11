---
title: Praca z DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416323"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="6c605-102">Praca z klasą DbContext</span><span class="sxs-lookup"><span data-stu-id="6c605-102">Working with DbContext</span></span>

<span data-ttu-id="6c605-103">Aby można było używać Entity Framework do wykonywania zapytań, wstawiania, aktualizowania i usuwania danych przy użyciu obiektów .NET, należy najpierw [utworzyć model](~/ef6/modeling/index.md) , który mapuje jednostki i relacje zdefiniowane w modelu do tabel w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6c605-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="6c605-104">Po utworzeniu modelu Klasa podstawowa, z którą współdziała aplikacja, jest `System.Data.Entity.DbContext` (często określana jako Klasa kontekstu).</span><span class="sxs-lookup"><span data-stu-id="6c605-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="6c605-105">Można użyć DbContext skojarzonego z modelem, aby:</span><span class="sxs-lookup"><span data-stu-id="6c605-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="6c605-106">Zapisywanie i wykonywanie zapytań</span><span class="sxs-lookup"><span data-stu-id="6c605-106">Write and execute queries</span></span>   
- <span data-ttu-id="6c605-107">Zmaterializowania wyniki zapytania jako obiekty jednostek</span><span class="sxs-lookup"><span data-stu-id="6c605-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="6c605-108">Śledzenie zmian wprowadzonych w tych obiektach</span><span class="sxs-lookup"><span data-stu-id="6c605-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="6c605-109">Utrwalanie zmian obiektów w bazie danych</span><span class="sxs-lookup"><span data-stu-id="6c605-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="6c605-110">Powiązywanie obiektów w kontrolkach z pamięcią</span><span class="sxs-lookup"><span data-stu-id="6c605-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="6c605-111">Na tej stronie przedstawiono wskazówki dotyczące zarządzania klasą kontekstową.</span><span class="sxs-lookup"><span data-stu-id="6c605-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="6c605-112">Definiowanie klasy pochodnej DbContext</span><span class="sxs-lookup"><span data-stu-id="6c605-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="6c605-113">Zalecanym sposobem pracy z kontekstem jest zdefiniowanie klasy, która dziedziczy z DbContext i uwidacznia właściwości Nieogólnymi, które reprezentują kolekcje określonych jednostek w kontekście.</span><span class="sxs-lookup"><span data-stu-id="6c605-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="6c605-114">Jeśli pracujesz z programem Dr Designer, kontekst zostanie wygenerowany dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="6c605-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="6c605-115">Jeśli pracujesz z Code First, zazwyczaj Napisz kontekst samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="6c605-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="6c605-116">Po umieszczeniu kontekstu należy wykonać zapytanie o, dodać (przy użyciu metod `Add` lub `Attach`) lub usunąć (przy użyciu `Remove`) jednostki w kontekście za pośrednictwem tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="6c605-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="6c605-117">Uzyskiwanie dostępu do właściwości `DbSet` w obiekcie kontekstu reprezentuje zapytanie początkowe zwracające wszystkie jednostki określonego typu.</span><span class="sxs-lookup"><span data-stu-id="6c605-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="6c605-118">Należy pamiętać, że tylko uzyskanie dostępu do właściwości nie spowoduje wykonania zapytania.</span><span class="sxs-lookup"><span data-stu-id="6c605-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="6c605-119">Zapytanie jest wykonywane, gdy:</span><span class="sxs-lookup"><span data-stu-id="6c605-119">A query is executed when:</span></span>  

- <span data-ttu-id="6c605-120">Jest on wyliczany przez instrukcję `foreach` (C#) lub `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="6c605-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="6c605-121">Jest on wyliczany przez operację kolekcji, taką jak `ToArray`, `ToDictionary`lub `ToList`.</span><span class="sxs-lookup"><span data-stu-id="6c605-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="6c605-122">Operatory LINQ, takie jak `First` lub `Any`, są określone w najbardziej zewnętrznej części zapytania.</span><span class="sxs-lookup"><span data-stu-id="6c605-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="6c605-123">Wywoływana jest jedna z następujących metod: `Load` Metoda rozszerzenia, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`i `DbSet<T>.Find`, jeśli jednostka z określonym kluczem nie została już załadowana w kontekście.</span><span class="sxs-lookup"><span data-stu-id="6c605-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="6c605-124">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="6c605-124">Lifetime</span></span>  

<span data-ttu-id="6c605-125">Okres istnienia kontekstu rozpoczyna się, gdy wystąpienie jest tworzone i kończące się, gdy wystąpienie zostanie usunięte lub nie zostało pobrane jako elementy bezużyteczne.</span><span class="sxs-lookup"><span data-stu-id="6c605-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="6c605-126">Użyj **polecenia using** , jeśli chcesz, aby wszystkie zasoby, które zostaną usunięte przez formant kontekstu na końcu bloku.</span><span class="sxs-lookup"><span data-stu-id="6c605-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="6c605-127">Gdy używasz **polecenia using**, kompilator automatycznie tworzy blok try/finally i wywołuje metodę Dispose w bloku **finally** .</span><span class="sxs-lookup"><span data-stu-id="6c605-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="6c605-128">Poniżej przedstawiono ogólne wytyczne dotyczące okresu istnienia kontekstu:</span><span class="sxs-lookup"><span data-stu-id="6c605-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="6c605-129">Podczas pracy z aplikacjami sieci Web Użyj wystąpienia kontekstu dla żądania.</span><span class="sxs-lookup"><span data-stu-id="6c605-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="6c605-130">Podczas pracy z Windows Presentation Foundation (WPF) lub Windows Forms, użyj wystąpienia kontekstu na formularz.</span><span class="sxs-lookup"><span data-stu-id="6c605-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="6c605-131">Dzięki temu można korzystać z funkcji śledzenia zmian udostępnianej przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="6c605-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="6c605-132">Jeśli wystąpienie kontekstu jest tworzone przez kontener iniekcji zależności, zwykle jest to odpowiedzialność za kontener do usunięcia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="6c605-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="6c605-133">Jeśli kontekst jest tworzony w kodzie aplikacji, należy pamiętać o usunięciu kontekstu, gdy nie jest już wymagany.</span><span class="sxs-lookup"><span data-stu-id="6c605-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="6c605-134">Podczas pracy z długotrwałym kontekstem należy wziąć pod uwagę następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="6c605-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="6c605-135">Podczas ładowania większej liczby obiektów i ich odwołań do pamięci, użycie pamięci przez kontekst może szybko ulec zwiększeniu.</span><span class="sxs-lookup"><span data-stu-id="6c605-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="6c605-136">Może to spowodować problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="6c605-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="6c605-137">Kontekst nie jest bezpieczny dla wątków, dlatego nie powinien być współużytkowany w wielu wątkach wykonujących wykonywane czynności.</span><span class="sxs-lookup"><span data-stu-id="6c605-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="6c605-138">Jeśli wyjątek powoduje, że kontekst jest w stanie niemożliwym do odzyskania, cała aplikacja może zakończyć działanie.</span><span class="sxs-lookup"><span data-stu-id="6c605-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="6c605-139">Prawdopodobieństwo uruchamiania w ramach problemów związanych z współbieżnością zwiększa się w miarę przerwy między czasem, gdy dane są badane i aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="6c605-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="6c605-140">Połączenia</span><span class="sxs-lookup"><span data-stu-id="6c605-140">Connections</span></span>  

<span data-ttu-id="6c605-141">Domyślnie, kontekst zarządza połączeniami z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="6c605-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="6c605-142">Kontekst otwiera i zamyka połączenia zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="6c605-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="6c605-143">Na przykład kontekst otwiera połączenie w celu wykonania zapytania, a następnie zamyka połączenie po przetworzeniu wszystkich zestawów wyników.</span><span class="sxs-lookup"><span data-stu-id="6c605-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="6c605-144">Istnieją przypadki, gdy chcesz mieć większą kontrolę nad tym, kiedy połączenie zostanie otwarte i zamknięte.</span><span class="sxs-lookup"><span data-stu-id="6c605-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="6c605-145">Na przykład podczas pracy z SQL Server Compact często zaleca się zachowanie oddzielnego otwartego połączenia z bazą danych przez okres istnienia aplikacji w celu zwiększenia wydajności.</span><span class="sxs-lookup"><span data-stu-id="6c605-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="6c605-146">Można zarządzać tym procesem ręcznie przy użyciu właściwości `Connection`.</span><span class="sxs-lookup"><span data-stu-id="6c605-146">You can manage this process manually by using the `Connection` property.</span></span>  
