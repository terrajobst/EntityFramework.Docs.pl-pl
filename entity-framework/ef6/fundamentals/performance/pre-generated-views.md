---
title: Wstępnie wygenerowane widoki mapowania — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419392"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="98d59-102">Wstępnie wygenerowane widoki mapowania</span><span class="sxs-lookup"><span data-stu-id="98d59-102">Pre-generated mapping views</span></span>
<span data-ttu-id="98d59-103">Zanim Entity Framework będzie mogła wykonać zapytanie lub zapisać zmiany w źródle danych, musi wygenerować zestaw widoków mapowania, aby uzyskać dostęp do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="98d59-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="98d59-104">Te widoki mapowania są zestawem instrukcji Entity SQL, które reprezentują bazę danych w sposób abstrakcyjny i są częścią metadanych, które są buforowane dla domeny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="98d59-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="98d59-105">Jeśli utworzysz wiele wystąpień tego samego kontekstu w tej samej domenie aplikacji, zostaną one ponownie użyte w widokach mapowania z buforowanych metadanych zamiast ich wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="98d59-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="98d59-106">Ponieważ generowanie widoku mapowania jest istotną częścią całkowitego kosztu wykonywania pierwszego zapytania, Entity Framework umożliwia wstępne Generowanie widoków mapowania i uwzględnianie ich w skompilowanym projekcie.</span><span class="sxs-lookup"><span data-stu-id="98d59-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="98d59-107"> Aby uzyskać więcej informacji, zobacz  [zagadnienia dotyczące wydajności (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="98d59-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="98d59-108">Generowanie widoków mapowania przy użyciu narzędzia EF PowerShell w wersji Community Edition</span><span class="sxs-lookup"><span data-stu-id="98d59-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="98d59-109">Najprostszym sposobem na wstępne wygenerowanie widoków jest użycie [Narzędzia Dr PowerShell Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="98d59-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="98d59-110">Po zainstalowaniu narzędzi do zarządzania narzędziami można korzystać z opcji menu do generowania widoków, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="98d59-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="98d59-111">Dla modeli **Code First** kliknij prawym przyciskiem myszy plik kodu, który zawiera klasę DbContext.</span><span class="sxs-lookup"><span data-stu-id="98d59-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="98d59-112">W przypadku modeli programu **Dr Designer** kliknij prawym przyciskiem myszy plik EDMX.</span><span class="sxs-lookup"><span data-stu-id="98d59-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![Generuj widoki](~/ef6/media/generateviews.png)

<span data-ttu-id="98d59-114">Po zakończeniu procesu będziesz mieć klasę podobną do następującej wygenerowanej</span><span class="sxs-lookup"><span data-stu-id="98d59-114">Once the process is finished you will have a class similar to the following generated</span></span>

![wygenerowane widoki](~/ef6/media/generatedviews.png)

<span data-ttu-id="98d59-116">Teraz po uruchomieniu aplikacji EF użyje tej klasy do załadowania widoków zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="98d59-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="98d59-117">Jeśli model zmieni się i nie utworzysz ponownie tej klasy, program EF zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="98d59-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="98d59-118">Generowanie widoków mapowania na podstawie kodu-EF6 lub nowszego</span><span class="sxs-lookup"><span data-stu-id="98d59-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="98d59-119">Innym sposobem generowania widoków jest użycie interfejsów API, które zapewnia program Dr.</span><span class="sxs-lookup"><span data-stu-id="98d59-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="98d59-120">Korzystając z tej metody, masz swobodę serializowania widoków, ale trzeba również samodzielnie załadować widoki.</span><span class="sxs-lookup"><span data-stu-id="98d59-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="98d59-121">**Ef6 tylko do wewnątrz** — interfejsy API przedstawione w tej sekcji zostały wprowadzone w Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="98d59-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="98d59-122">Jeśli używasz wcześniejszej wersji, te informacje nie są stosowane.</span><span class="sxs-lookup"><span data-stu-id="98d59-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="98d59-123">Generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="98d59-123">Generating Views</span></span>

<span data-ttu-id="98d59-124">Interfejsy API do generowania widoków znajdują się w klasie System. Data. Entity. Core. Mapping. StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="98d59-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="98d59-125">Można pobrać StorageMappingCollection dla kontekstu przy użyciu obiektu MetadataWorkspace obiektu ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="98d59-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="98d59-126">Jeśli używasz nowszego interfejsu API DbContext, możesz uzyskać do niego dostęp przy użyciu IObjectContextAdapter, takiego jak poniżej, w tym kodzie mamy wystąpienie pochodne DbContext o nazwie DbContext:</span><span class="sxs-lookup"><span data-stu-id="98d59-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="98d59-127">Po udostępnieniu StorageMappingItemCollection można uzyskać dostęp do metod GenerateViews i ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="98d59-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="98d59-128">Pierwsza metoda tworzy słownik z wpisem dla każdego widoku w mapowaniu kontenera.</span><span class="sxs-lookup"><span data-stu-id="98d59-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="98d59-129">Druga metoda oblicza wartość skrótu dla mapowania jednego kontenera i jest używana w czasie wykonywania w celu sprawdzenia, czy model nie został zmieniony od czasu, gdy widoki zostały wstępnie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="98d59-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="98d59-130">Zastępowanie dwóch metod są dostępne dla złożonych scenariuszy obejmujących wiele mapowań kontenerów.</span><span class="sxs-lookup"><span data-stu-id="98d59-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="98d59-131">Podczas generowania widoków zostanie wywołana metoda GenerateViews, a następnie nastąpi wygenerowanie wyników EntitySetBase i DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="98d59-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="98d59-132">Należy również przechowywać skrót wygenerowany przez metodę ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="98d59-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="98d59-133">Ładowanie widoków</span><span class="sxs-lookup"><span data-stu-id="98d59-133">Loading Views</span></span>

<span data-ttu-id="98d59-134">Aby załadować widoki wygenerowane przez metodę GenerateViews, można dostarczyć EF z klasą, która dziedziczy z klasy abstrakcyjnej DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="98d59-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="98d59-135">DbMappingViewCache określa dwie metody, które należy zaimplementować:</span><span class="sxs-lookup"><span data-stu-id="98d59-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="98d59-136">Właściwość MappingHashValue musi zwracać skrót wygenerowany przez metodę ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="98d59-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="98d59-137">Gdy program Dr będzie pytał o widoki, najpierw generuje i porówna wartość skrótu modelu z skrótem zwracanym przez tę właściwość.</span><span class="sxs-lookup"><span data-stu-id="98d59-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="98d59-138">Jeśli nie są zgodne, EF zgłosi wyjątek EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="98d59-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="98d59-139">Metoda GetView akceptuje EntitySetBase i należy zwrócić element DbMappingVIew zawierający EntitySql, który został wygenerowany dla, który został skojarzony z daną EntitySetBase w słowniku wygenerowanym przez metodę GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="98d59-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="98d59-140">Jeśli Dr pyta o widok, którego nie masz, wówczas GetView powinien zwrócić wartość null.</span><span class="sxs-lookup"><span data-stu-id="98d59-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="98d59-141">Poniżej znajduje się wyciąg z DbMappingViewCache, który jest generowany przy użyciu narzędzi, zgodnie z powyższym opisem. w tym artykule zobaczymy jeden ze sposobów przechowywania i pobierania wymaganego EntitySql.</span><span class="sxs-lookup"><span data-stu-id="98d59-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="98d59-142">Aby można było korzystać z DbMappingViewCache, należy dodać użycie DbMappingViewCacheTypeAttribute, określając kontekst, dla którego został utworzony.</span><span class="sxs-lookup"><span data-stu-id="98d59-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="98d59-143">W kodzie poniżej skojarzemy BlogContext z klasą MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="98d59-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="98d59-144">W przypadku bardziej złożonych scenariuszy wystąpienia pamięci podręcznej widoku mapowania można podać, określając fabrykę pamięci podręcznej widoku mapowania.</span><span class="sxs-lookup"><span data-stu-id="98d59-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="98d59-145">Można to zrobić przez zaimplementowanie klasy abstrakcyjnej System. Data. Entity. Infrastructure. MappingViews. DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="98d59-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="98d59-146">Wystąpienie używanej fabryki pamięci podręcznej widoku mapowania można pobrać lub ustawić za pomocą StorageMappingItemCollection. MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="98d59-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
