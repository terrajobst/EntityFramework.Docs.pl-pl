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
# <a name="pre-generated-mapping-views"></a>Wstępnie wygenerowane widoki mapowania
Zanim Entity Framework będzie mogła wykonać zapytanie lub zapisać zmiany w źródle danych, musi wygenerować zestaw widoków mapowania, aby uzyskać dostęp do bazy danych. Te widoki mapowania są zestawem instrukcji Entity SQL, które reprezentują bazę danych w sposób abstrakcyjny i są częścią metadanych, które są buforowane dla domeny aplikacji. Jeśli utworzysz wiele wystąpień tego samego kontekstu w tej samej domenie aplikacji, zostaną one ponownie użyte w widokach mapowania z buforowanych metadanych zamiast ich wygenerowania. Ponieważ generowanie widoku mapowania jest istotną częścią całkowitego kosztu wykonywania pierwszego zapytania, Entity Framework umożliwia wstępne Generowanie widoków mapowania i uwzględnianie ich w skompilowanym projekcie. Aby uzyskać więcej informacji, zobacz  [zagadnienia dotyczące wydajności (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Generowanie widoków mapowania przy użyciu narzędzia EF PowerShell w wersji Community Edition

Najprostszym sposobem na wstępne wygenerowanie widoków jest użycie [Narzędzia Dr PowerShell Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Po zainstalowaniu narzędzi do zarządzania narzędziami można korzystać z opcji menu do generowania widoków, jak pokazano poniżej.

-   Dla modeli **Code First** kliknij prawym przyciskiem myszy plik kodu, który zawiera klasę DbContext.
-   W przypadku modeli programu **Dr Designer** kliknij prawym przyciskiem myszy plik EDMX.

![Generuj widoki](~/ef6/media/generateviews.png)

Po zakończeniu procesu będziesz mieć klasę podobną do następującej wygenerowanej

![wygenerowane widoki](~/ef6/media/generatedviews.png)

Teraz po uruchomieniu aplikacji EF użyje tej klasy do załadowania widoków zgodnie z potrzebami. Jeśli model zmieni się i nie utworzysz ponownie tej klasy, program EF zgłosi wyjątek.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Generowanie widoków mapowania na podstawie kodu-EF6 lub nowszego

Innym sposobem generowania widoków jest użycie interfejsów API, które zapewnia program Dr. Korzystając z tej metody, masz swobodę serializowania widoków, ale trzeba również samodzielnie załadować widoki.

> [!NOTE]
> **Ef6 tylko do wewnątrz** — interfejsy API przedstawione w tej sekcji zostały wprowadzone w Entity Framework 6. Jeśli używasz wcześniejszej wersji, te informacje nie są stosowane.

### <a name="generating-views"></a>Generowanie widoków

Interfejsy API do generowania widoków znajdują się w klasie System. Data. Entity. Core. Mapping. StorageMappingItemCollection. Można pobrać StorageMappingCollection dla kontekstu przy użyciu obiektu MetadataWorkspace obiektu ObjectContext. Jeśli używasz nowszego interfejsu API DbContext, możesz uzyskać do niego dostęp przy użyciu IObjectContextAdapter, takiego jak poniżej, w tym kodzie mamy wystąpienie pochodne DbContext o nazwie DbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Po udostępnieniu StorageMappingItemCollection można uzyskać dostęp do metod GenerateViews i ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

Pierwsza metoda tworzy słownik z wpisem dla każdego widoku w mapowaniu kontenera. Druga metoda oblicza wartość skrótu dla mapowania jednego kontenera i jest używana w czasie wykonywania w celu sprawdzenia, czy model nie został zmieniony od czasu, gdy widoki zostały wstępnie wygenerowane. Zastępowanie dwóch metod są dostępne dla złożonych scenariuszy obejmujących wiele mapowań kontenerów.

Podczas generowania widoków zostanie wywołana metoda GenerateViews, a następnie nastąpi wygenerowanie wyników EntitySetBase i DbMappingView. Należy również przechowywać skrót wygenerowany przez metodę ComputeMappingHashValue.

### <a name="loading-views"></a>Ładowanie widoków

Aby załadować widoki wygenerowane przez metodę GenerateViews, można dostarczyć EF z klasą, która dziedziczy z klasy abstrakcyjnej DbMappingViewCache. DbMappingViewCache określa dwie metody, które należy zaimplementować:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Właściwość MappingHashValue musi zwracać skrót wygenerowany przez metodę ComputeMappingHashValue. Gdy program Dr będzie pytał o widoki, najpierw generuje i porówna wartość skrótu modelu z skrótem zwracanym przez tę właściwość. Jeśli nie są zgodne, EF zgłosi wyjątek EntityCommandCompilationException.

Metoda GetView akceptuje EntitySetBase i należy zwrócić element DbMappingVIew zawierający EntitySql, który został wygenerowany dla, który został skojarzony z daną EntitySetBase w słowniku wygenerowanym przez metodę GenerateViews. Jeśli Dr pyta o widok, którego nie masz, wówczas GetView powinien zwrócić wartość null.

Poniżej znajduje się wyciąg z DbMappingViewCache, który jest generowany przy użyciu narzędzi, zgodnie z powyższym opisem. w tym artykule zobaczymy jeden ze sposobów przechowywania i pobierania wymaganego EntitySql.

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

Aby można było korzystać z DbMappingViewCache, należy dodać użycie DbMappingViewCacheTypeAttribute, określając kontekst, dla którego został utworzony. W kodzie poniżej skojarzemy BlogContext z klasą MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

W przypadku bardziej złożonych scenariuszy wystąpienia pamięci podręcznej widoku mapowania można podać, określając fabrykę pamięci podręcznej widoku mapowania. Można to zrobić przez zaimplementowanie klasy abstrakcyjnej System. Data. Entity. Infrastructure. MappingViews. DbMappingViewCacheFactory. Wystąpienie używanej fabryki pamięci podręcznej widoku mapowania można pobrać lub ustawić za pomocą StorageMappingItemCollection. MappingViewCacheFactoryproperty.
