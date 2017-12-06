---
title: "Trwa ładowanie powiązanych danych - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="f82a0-102">Trwa ładowanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="f82a0-102">Loading Related Data</span></span>

<span data-ttu-id="f82a0-103">Program Entity Framework Core umożliwia przy użyciu właściwości nawigacji w modelu załadować powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="f82a0-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="f82a0-104">Istnieją trzy typowe wzorce O/RM używana do załadowania powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="f82a0-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="f82a0-105">**Ładowanie wczesny** oznacza, że powiązane dane są ładowane z bazy danych w ramach początkowego zapytania.</span><span class="sxs-lookup"><span data-stu-id="f82a0-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="f82a0-106">**Ładowanie jawne** oznacza, że powiązane jest jawnie załadować danych z bazy danych w późniejszym czasie.</span><span class="sxs-lookup"><span data-stu-id="f82a0-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="f82a0-107">**Powolne ładowanie** oznacza, że pokrewne niewidocznie załadowaniu danych z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f82a0-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="f82a0-108">Powolne ładowanie nie jest jeszcze możliwe EF podstawowych.</span><span class="sxs-lookup"><span data-stu-id="f82a0-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="f82a0-109">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="f82a0-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="f82a0-110">Ładowanie wczesny</span><span class="sxs-lookup"><span data-stu-id="f82a0-110">Eager loading</span></span>

<span data-ttu-id="f82a0-111">Można użyć `Include` metodę, aby określić powiązane dane mają zostać uwzględnione w wynikach zapytania.</span><span class="sxs-lookup"><span data-stu-id="f82a0-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="f82a0-112">W poniższym przykładzie, blogów, z którego są zwracane w wynikach będzie miał ich `Posts` właściwości wypełniane przy użyciu powiązanych wpisów.</span><span class="sxs-lookup"><span data-stu-id="f82a0-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="f82a0-113">Entity Framework Core będzie automatycznie konfigurowania właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu.</span><span class="sxs-lookup"><span data-stu-id="f82a0-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f82a0-114">Dlatego nawet wtedy, gdy wyraźnie nie zawierają danych dla właściwości nawigacji, właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.</span><span class="sxs-lookup"><span data-stu-id="f82a0-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="f82a0-115">Powiązane dane z wielu relacji można uwzględnić w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="f82a0-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="f82a0-116">W tym wiele poziomów</span><span class="sxs-lookup"><span data-stu-id="f82a0-116">Including multiple levels</span></span>

<span data-ttu-id="f82a0-117">Można przejść do relacji z obejmują wiele poziomów powiązanych danych przy użyciu `ThenInclude` metody.</span><span class="sxs-lookup"><span data-stu-id="f82a0-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="f82a0-118">Poniższy przykład ładuje wszystkie blogi, ich powiązane ogłoszeń i autor każdego post.</span><span class="sxs-lookup"><span data-stu-id="f82a0-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="f82a0-119">Tworzenia łańcucha wielu wywołań `ThenInclude` aby kontynuować, tym więcej poziomy powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="f82a0-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="f82a0-120">Możesz łączyć wszystko to zostać uwzględnione w jednym zapytaniu powiązane dane z różnych poziomach i wielu elementów głównych.</span><span class="sxs-lookup"><span data-stu-id="f82a0-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="f82a0-121">Można dołączyć wielu powiązanych jednostek dla jednego z jednostkami, które ma być.</span><span class="sxs-lookup"><span data-stu-id="f82a0-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="f82a0-122">Na przykład podczas wykonywania zapytania `Blog`s, obejmują `Posts` , a następnie chcesz dołączyć zarówno `Author` i `Tags` z `Posts`.</span><span class="sxs-lookup"><span data-stu-id="f82a0-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="f82a0-123">Aby to zrobić, należy określić każdy obejmować ścieżki od katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="f82a0-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="f82a0-124">Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="f82a0-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="f82a0-125">Oznacza to, że otrzymasz nadmiarowe sprzężeń, w większości przypadków będzie skonsolidować EF sprzężeń podczas generowania SQL.</span><span class="sxs-lookup"><span data-stu-id="f82a0-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="f82a0-126">Ignorowane obejmuje</span><span class="sxs-lookup"><span data-stu-id="f82a0-126">Ignored includes</span></span>

<span data-ttu-id="f82a0-127">Jeśli zmienisz zapytanie tak, aby nie będzie już zwracać wystąpień typów jednostek, które rozpoczął się zapytanie, operatorów include są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="f82a0-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="f82a0-128">W poniższym przykładzie operatory dołączania są oparte na `Blog`, ale następnie `Select` operator służy do zmiany zwracanego typu anonimowego przez zapytanie.</span><span class="sxs-lookup"><span data-stu-id="f82a0-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="f82a0-129">W takim przypadku operatory include nie obowiązują.</span><span class="sxs-lookup"><span data-stu-id="f82a0-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="f82a0-130">Domyślnie EF Core będzie rejestrować ostrzeżenie podczas obejmują operatory są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="f82a0-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="f82a0-131">Zobacz [rejestrowanie](../miscellaneous/logging.md) Aby uzyskać więcej informacji o wyświetlaniu dane wyjściowe rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="f82a0-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="f82a0-132">Można zmienić to zachowanie, gdy operator include jest ignorowana, aby zgłosić lub nie rób.</span><span class="sxs-lookup"><span data-stu-id="f82a0-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="f82a0-133">Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` Jeśli używasz platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f82a0-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="f82a0-134">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="f82a0-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="f82a0-135">Ta funkcja została wprowadzona w EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="f82a0-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="f82a0-136">Można w sposób jawny załadować właściwości nawigacji za pośrednictwem `DbContext.Entry(...)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f82a0-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="f82a0-137">Można również jawnie załadować właściwość nawigacji, wykonując oddzielne zapytania, które zwraca powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="f82a0-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="f82a0-138">Jeśli jest włączone śledzenie zmian, podczas ładowania jednostki, EF Core zostanie automatycznie Ustaw właściwości nawigacji entitiy nowo załadowanych do odwoływania się do żadnych jednostek już załadowany, a następnie ustaw właściwości nawigacji jednostki już załadowany do odwoływania się do nowo załadowanych jednostki.</span><span class="sxs-lookup"><span data-stu-id="f82a0-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="f82a0-139">Wykonywanie zapytania powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="f82a0-139">Querying related entities</span></span>

<span data-ttu-id="f82a0-140">Można również uzyskać kwerendy LINQ, która reprezentuje zawartość właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f82a0-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="f82a0-141">Pozwala na wykonywanie czynności, takie jak uruchomienie operatora agregacji za pośrednictwem powiązanych jednostek bez ładowania ich do pamięci.</span><span class="sxs-lookup"><span data-stu-id="f82a0-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="f82a0-142">Można również filtrować, które powiązanych jednostek są ładowane do pamięci.</span><span class="sxs-lookup"><span data-stu-id="f82a0-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="f82a0-143">powolne ładowanie</span><span class="sxs-lookup"><span data-stu-id="f82a0-143">Lazy loading</span></span>

<span data-ttu-id="f82a0-144">Powolne ładowanie nie jest jeszcze obsługiwany przez EF rdzeń.</span><span class="sxs-lookup"><span data-stu-id="f82a0-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="f82a0-145">Możesz wyświetlić [elementu opóźnionego ładowania na naszych zaległości](https://github.com/aspnet/EntityFramework/issues/3797) do śledzenia tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="f82a0-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="f82a0-146">Powiązane dane i serializacja</span><span class="sxs-lookup"><span data-stu-id="f82a0-146">Related data and serialization</span></span>

<span data-ttu-id="f82a0-147">Ponieważ właściwości nawigacji automatycznie konfigurowania będzie EF Core, można na końcu cykli na wykresie obiektu.</span><span class="sxs-lookup"><span data-stu-id="f82a0-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="f82a0-148">Na przykład ładowanie blogu i powiązanych spowoduje wpisów w obiekcie blog, który odwołuje się do kolekcji wpisów.</span><span class="sxs-lookup"><span data-stu-id="f82a0-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="f82a0-149">Każdy z tych stanowisk ma odwołanie do blogu.</span><span class="sxs-lookup"><span data-stu-id="f82a0-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="f82a0-150">Niektórych struktur serializacji nie zezwalają na takie cykle.</span><span class="sxs-lookup"><span data-stu-id="f82a0-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="f82a0-151">Na przykład struktury Json.NET zgłosi następujący wyjątek, jeśli encoutered cyklu.</span><span class="sxs-lookup"><span data-stu-id="f82a0-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="f82a0-152">Newtonsoft.Json.JsonSerializationException: Self odwołujące się do pętli wykryto dla właściwości "Blog" z typem "MyApplication.Models.Blog".</span><span class="sxs-lookup"><span data-stu-id="f82a0-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="f82a0-153">Jeśli używasz platformy ASP.NET Core, można skonfigurować struktury Json.NET do ignorowania cykle znalezionych w grafie obiektu.</span><span class="sxs-lookup"><span data-stu-id="f82a0-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="f82a0-154">Jest to wykonywane w `ConfigureServices(...)` metoda `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="f82a0-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
