---
title: Widoki wstępnie wygenerowanego mapowania — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: da5d59ba5a899a0ee3a1eec3db0da1b4ece871d8
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284112"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="cfdbb-102">Widoki wstępnie wygenerowanego mapowania</span><span class="sxs-lookup"><span data-stu-id="cfdbb-102">Pre-generated mapping views</span></span>
<span data-ttu-id="cfdbb-103">Zanim Entity Framework można wykonać zapytania i zapisać zmiany w źródle danych, go wygenerować zestaw widoków mapowanie dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="cfdbb-104">Widoki te mapowania są zbiór instrukcję SQL jednostki, która reprezentuje bazy danych w sposób abstrakcyjnej i są częścią metadanych, które są buforowane dla domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="cfdbb-105">Jeśli tworzysz wiele wystąpień tego samego kontekstu w tej samej domenie aplikacji, będą ponownie używały widokach mapowania z pamięci podręcznej metadanych zamiast ponownego generowania ich.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="cfdbb-106">Ponieważ generowanie Widok mapowania jest znaczna część całkowity koszt wykonania pierwszego zapytania, platformy Entity Framework umożliwia wstępnie wygenerować widokach mapowania i umieścić je w projekcie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="cfdbb-107">Aby uzyskać więcej informacji, zobacz [zagadnienia związane z wydajnością (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="cfdbb-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="cfdbb-108">Generowanie mapowanie widoków z zaawansowanych narzędzi EF</span><span class="sxs-lookup"><span data-stu-id="cfdbb-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="cfdbb-109">Najłatwiej można wstępnie wygenerować widoków jest użycie [EF Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="cfdbb-109">The easiest way to pre-generate views is to use the [EF Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="cfdbb-110">Po utworzeniu zainstalowano narzędzia zasilania masz opcję menu, aby wygenerować widoki, zgodnie z poniższymi instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="cfdbb-111">Aby uzyskać **Code First** modeli kliknij prawym przyciskiem myszy plik kodu zawierający klasy DbContext.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="cfdbb-112">Aby uzyskać **projektancie platformy EF** modeli kliknij prawym przyciskiem myszy w pliku EDMX.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![Generowanie widoków](~/ef6/media/generateviews.png)

<span data-ttu-id="cfdbb-114">Po zakończeniu procesu będą dostępne podobny do następującego wygenerowane klasy</span><span class="sxs-lookup"><span data-stu-id="cfdbb-114">Once the process is finished you will have a class similar to the following generated</span></span>

![wygenerowanych widoków](~/ef6/media/generatedviews.png)

<span data-ttu-id="cfdbb-116">Teraz po uruchomieniu aplikacji EF użyje tej klasy można załadować widoki, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="cfdbb-117">W przypadku zmiany modelu i ponownie generuje ta klasa EF spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="cfdbb-118">Generowanie widoków mapowania z kodu — od wersji EF6</span><span class="sxs-lookup"><span data-stu-id="cfdbb-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="cfdbb-119">Inny sposób, aby wygenerować widoków jest korzystanie z interfejsów API, który zapewnia EF.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="cfdbb-120">Przy użyciu tej metody należy swobody serializacji widoków, jednak lubisz, ale trzeba będzie również załadować widoków samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="cfdbb-121">**EF6 począwszy tylko** — interfejsy API, przedstawione w tej sekcji zostały wprowadzone w programie Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="cfdbb-122">Jeśli używasz starszej wersji tych informacji nie ma zastosowania.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="cfdbb-123">Generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="cfdbb-123">Generating Views</span></span>

<span data-ttu-id="cfdbb-124">Interfejsy API, aby wygenerować widoki znajdują się w klasie System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="cfdbb-125">Aby pobrać StorageMappingCollection dla kontekstu, za pomocą obiektu MetadataWorkspace dla obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="cfdbb-126">Jeśli używasz nowszej interfejsu API typu DbContext, wówczas użytkownik może uzyskać dostępu do tego za pomocą IObjectContextAdapter, takich jak poniżej, w tym kodzie mamy wystąpienie usługi DbContext pochodnej wywołuje dbContext:</span><span class="sxs-lookup"><span data-stu-id="cfdbb-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="cfdbb-127">Po utworzeniu obiekt StorageMappingItemCollection można uzyskać dostęp do metod GenerateViews i ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="cfdbb-128">Pierwsza metoda tworzy słownik z wpis dla każdego widoku w mapowaniu kontenera.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="cfdbb-129">Druga metoda oblicza wartość skrótu dla mapowania jeden kontener i jest używany w czasie wykonywania do sprawdzania poprawności, model nie zmienił się od czasu widoki zostały wstępnie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="cfdbb-130">Zastąpienia dwie metody są udostępniane dla złożonych scenariuszy obejmujących wiele mapowań kontenera.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="cfdbb-131">Podczas generowania widoków spowoduje wywołanie metody GenerateViews i następnie zapisać wynikowy EntitySetBase i DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="cfdbb-132">Należy również przechowywać wyznaczania wartości skrótu, generowany przez metodę ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="cfdbb-133">Trwa ładowanie widoków</span><span class="sxs-lookup"><span data-stu-id="cfdbb-133">Loading Views</span></span>

<span data-ttu-id="cfdbb-134">Aby załadować widoki generowane przez metodę GenerateViews, możesz zapewnić EF klasę, która dziedziczy z klasy abstrakcyjnej DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="cfdbb-135">DbMappingViewCache określa dwie metody, które należy zaimplementować:</span><span class="sxs-lookup"><span data-stu-id="cfdbb-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="cfdbb-136">Właściwość MappingHashValue musi zwracać wyznaczania wartości skrótu, generowany przez metodę ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="cfdbb-137">Gdy EF jest przejście do zadania dla widoków najpierw wygeneruje i porównania wartości skrótu modelu przy użyciu skrótu zwracane przez tę właściwość.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="cfdbb-138">Jeśli nie są zgodne EF spowoduje zgłoszenie wyjątku EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="cfdbb-139">Metoda GetView będzie akceptować EntitySetBase i muszą zwracać DbMappingVIew, zawierający EntitySql, który został wygenerowany w tym został skojarzony z danym EntitySetBase w słowniku generowany przez metodę GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="cfdbb-140">Jeśli EF prosi o widoku, że nie masz następnie GetView powinna zwrócić wartość null.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="cfdbb-141">Poniżej przedstawiono wyciąg z DbMappingViewCache, który jest generowany przy użyciu zaawansowanych narzędzi, jak opisano powyżej, w nim widać sposób przechowywania i pobierania EntitySql wymagane.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="cfdbb-142">Mają zastosowanie do programów EF swoje DbMappingViewCache dodasz Użyj DbMappingViewCacheTypeAttribute, określając kontekst, który został utworzony dla.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="cfdbb-143">W poniższym kodzie za pomocą klasy MyMappingViewCache wnoszonym BlogContext.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="cfdbb-144">W przypadku bardziej złożonych scenariuszy wystąpienia pamięci podręcznej Widok mapowania można podać, określając fabrykę pamięci podręcznej Widok mapowania.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="cfdbb-145">Można to zrobić poprzez implementację klasy abstrakcyjnej System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="cfdbb-146">Wystąpienie fabryki pamięci podręcznej Widok mapowania, która jest używana można pobrać lub ustawić za pomocą StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="cfdbb-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
