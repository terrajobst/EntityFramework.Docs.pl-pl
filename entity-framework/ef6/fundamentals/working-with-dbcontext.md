---
title: Praca z DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912047"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="47362-102">Praca z typu DbContext</span><span class="sxs-lookup"><span data-stu-id="47362-102">Working with DbContext</span></span>

<span data-ttu-id="47362-103">Aby można było używać programu Entity Framework do zapytania, wstawiania, aktualizowania i usuwania danych, używając obiektów platformy .NET, należy najpierw [utworzyć Model](~/ef6/modeling/index.md) która mapuje jednostek i relacji, które są zdefiniowane w modelu służącym do tabel w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="47362-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="47362-104">Po utworzeniu modelu, jest podstawową klasą aplikacji współdziała z `System.Data.Entity.DbContext` (często określanymi jako klasy kontekstu).</span><span class="sxs-lookup"><span data-stu-id="47362-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="47362-105">Można użyć typu DbContext powiązanych z modelu do:</span><span class="sxs-lookup"><span data-stu-id="47362-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="47362-106">Pisanie i wykonywania zapytań</span><span class="sxs-lookup"><span data-stu-id="47362-106">Write and execute queries</span></span>   
- <span data-ttu-id="47362-107">Zmaterializowania wyników zapytania jako obiekty jednostki</span><span class="sxs-lookup"><span data-stu-id="47362-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="47362-108">Śledź zmiany wprowadzone do tych obiektów</span><span class="sxs-lookup"><span data-stu-id="47362-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="47362-109">Utrwalanie zmian obiektów na bazie danych</span><span class="sxs-lookup"><span data-stu-id="47362-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="47362-110">Powiązanie obiektów w pamięci na kontrolkę interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="47362-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="47362-111">Ta strona udostępnia wskazówki na temat zarządzania z klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="47362-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="47362-112">Definiowanie klasy pochodne typu DbContext</span><span class="sxs-lookup"><span data-stu-id="47362-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="47362-113">Zalecany sposób pracy z kontekstu jest Definiowanie klasy, która pochodzi od typu DbContext i udostępnia właściwości DbSet, które reprezentują kolekcji określonej jednostki w kontekście.</span><span class="sxs-lookup"><span data-stu-id="47362-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="47362-114">Jeśli pracujesz w Projektancie platformy EF kontekście zostanie wygenerowany dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="47362-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="47362-115">Jeśli pracujesz z Code First, zwykle napiszesz kontekście samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="47362-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="47362-116">Po utworzeniu kontekstu, należy wyszukać, Dodaj (przy użyciu `Add` lub `Attach` metody) lub Usuń (przy użyciu `Remove`) jednostki w kontekście za pomocą tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="47362-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="47362-117">Uzyskiwanie dostępu do `DbSet` właściwość obiektu kontekstu reprezentują począwszy od zapytania, które zwraca wszystkich jednostek określonego typu.</span><span class="sxs-lookup"><span data-stu-id="47362-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="47362-118">Należy zauważyć, że tylko dostęp do właściwości nie będą wykonywane zapytania.</span><span class="sxs-lookup"><span data-stu-id="47362-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="47362-119">Zapytanie jest wykonywane gdy:</span><span class="sxs-lookup"><span data-stu-id="47362-119">A query is executed when:</span></span>  

- <span data-ttu-id="47362-120">Są wyliczane przez `foreach` (C#) lub `For Each` — instrukcja (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="47362-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="47362-121">Jego są wyliczane przez operację kolekcji takich jak `ToArray`, `ToDictionary`, lub `ToList`.</span><span class="sxs-lookup"><span data-stu-id="47362-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="47362-122">Operatory LINQ, takich jak `First` lub `Any` są określone w najbardziej zewnętrznej część zapytania.</span><span class="sxs-lookup"><span data-stu-id="47362-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="47362-123">Noszą nazwę jednej z następujących metod: `Load` metody rozszerzenia `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, i `DbSet<T>.Find`, jeśli jednostki z określonym kluczem nie znajduje się już ładowane w kontekście.</span><span class="sxs-lookup"><span data-stu-id="47362-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="47362-124">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="47362-124">Lifetime</span></span>  

<span data-ttu-id="47362-125">Okres istnienia w kontekście rozpoczyna się, gdy wystąpienie zostanie utworzona i kończy się, gdy wystąpienie jest usunięty lub zebranych elementów bezużytecznych.</span><span class="sxs-lookup"><span data-stu-id="47362-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="47362-126">Użyj **przy użyciu** Jeśli chcesz, aby wszystkie zasoby, które kontekstu kontrolki usuwana na końcu bloku.</span><span class="sxs-lookup"><span data-stu-id="47362-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="47362-127">Kiedy używasz **przy użyciu**, kompilator automatycznie tworzy blok try/finally i wywołuje metodę dispose w **na koniec** bloku.</span><span class="sxs-lookup"><span data-stu-id="47362-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="47362-128">Poniżej przedstawiono ogólne wskazówki przy podejmowaniu decyzji o okresie istnienia w kontekście:</span><span class="sxs-lookup"><span data-stu-id="47362-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="47362-129">Podczas pracy z aplikacjami sieci Web, należy użyć wystąpienia kontekstu na żądanie.</span><span class="sxs-lookup"><span data-stu-id="47362-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="47362-130">Podczas pracy z Windows Presentation Foundation (WPF) lub Windows Forms, należy użyć wystąpienia kontekstu dla formularza.</span><span class="sxs-lookup"><span data-stu-id="47362-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="47362-131">Dzięki temu można korzystać z funkcji śledzenia zmian zapewnia tego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="47362-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="47362-132">Jeśli wystąpienie kontekstu jest tworzony przez kontener iniekcji zależności, zazwyczaj przyczyną jest odpowiedzialność kontenera można zlikwidować kontekstu.</span><span class="sxs-lookup"><span data-stu-id="47362-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="47362-133">Jeśli kontekst jest tworzony w kodzie aplikacji, pamiętaj, aby dysponować kontekstu, gdy nie jest już wymagany.</span><span class="sxs-lookup"><span data-stu-id="47362-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="47362-134">Podczas pracy z kontekstem długoterminowych należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="47362-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="47362-135">Jak załadować więcej obiektów i ich odwołania do pamięci, zmniejszenie zużycia pamięci kontekstu może szybko wzrosnąć.</span><span class="sxs-lookup"><span data-stu-id="47362-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="47362-136">Może to spowodować problemy z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="47362-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="47362-137">Kontekst nie jest bezpieczna dla wątków, w związku z tym go nie może być współużytkowany przez wiele wątków jednocześnie wykonywania pracy.</span><span class="sxs-lookup"><span data-stu-id="47362-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="47362-138">Jeśli wyjątek powoduje, że kontekst będzie w stanie nieodwracalny, może rozwiązać niniejszą całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="47362-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="47362-139">Ryzyko związane z współbieżności problemy podczas zwiększyć wraz ze wzrostem natężenia przerwy między czasem, gdy dane są badane i zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="47362-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="47362-140">Połączenia</span><span class="sxs-lookup"><span data-stu-id="47362-140">Connections</span></span>  

<span data-ttu-id="47362-141">Domyślnie w kontekście zarządza połączeń z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="47362-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="47362-142">Kontekst otwiera i zamyka połączenia, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="47362-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="47362-143">Na przykład kontekście otwiera połączenie, aby wykonać zapytanie, a następnie zamyka połączenia, po przetworzeniu wszystkich zestawów wyników.</span><span class="sxs-lookup"><span data-stu-id="47362-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="47362-144">Istnieją przypadki, gdy użytkownik chce mieć większą kontrolę nad, gdy połączenie otwiera i zamyka.</span><span class="sxs-lookup"><span data-stu-id="47362-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="47362-145">Na przykład podczas pracy z programu SQL Server Compact, często zaleca się zachować oddzielne Otwieranie połączenia z bazą danych dla cyklu życia aplikacji w celu zwiększenia wydajności.</span><span class="sxs-lookup"><span data-stu-id="47362-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="47362-146">Ten proces można zarządzać ręcznie za pomocą `Connection` właściwości.</span><span class="sxs-lookup"><span data-stu-id="47362-146">You can manage this process manually by using the `Connection` property.</span></span>  
