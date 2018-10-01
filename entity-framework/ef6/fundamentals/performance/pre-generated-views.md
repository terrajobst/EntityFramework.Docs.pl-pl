---
title: Widoki wstępnie wygenerowanego mapowania — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459529"
---
# <a name="pre-generated-mapping-views"></a>Widoki wstępnie wygenerowanego mapowania
Zanim Entity Framework można wykonać zapytania i zapisać zmiany w źródle danych, go wygenerować zestaw widoków mapowanie dostępu do bazy danych. Widoki te mapowania są zbiór instrukcję SQL jednostki, która reprezentuje bazy danych w sposób abstrakcyjnej i są częścią metadanych, które są buforowane dla domeny aplikacji. Jeśli tworzysz wiele wystąpień tego samego kontekstu w tej samej domenie aplikacji, będą ponownie używały widokach mapowania z pamięci podręcznej metadanych zamiast ponownego generowania ich. Ponieważ generowanie Widok mapowania jest znaczna część całkowity koszt wykonania pierwszego zapytania, platformy Entity Framework umożliwia wstępnie wygenerować widokach mapowania i umieścić je w projekcie skompilowany. Aby uzyskać więcej informacji, zobacz [zagadnienia związane z wydajnością (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Generowanie mapowanie widoków przy użyciu programu EF Power Tools Community Edition

Najłatwiej można wstępnie wygenerować widoków jest użycie [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Po utworzeniu zainstalowano narzędzia zasilania masz opcję menu, aby wygenerować widoki, zgodnie z poniższymi instrukcjami.

-   Aby uzyskać **Code First** modeli kliknij prawym przyciskiem myszy plik kodu zawierający klasy DbContext.
-   Aby uzyskać **projektancie platformy EF** modeli kliknij prawym przyciskiem myszy w pliku EDMX.

![Generowanie widoków](~/ef6/media/generateviews.png)

Po zakończeniu procesu będą dostępne podobny do następującego wygenerowane klasy

![wygenerowanych widoków](~/ef6/media/generatedviews.png)

Teraz po uruchomieniu aplikacji EF użyje tej klasy można załadować widoki, zgodnie z potrzebami. W przypadku zmiany modelu i ponownie generuje ta klasa EF spowoduje zgłoszenie wyjątku.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Generowanie widoków mapowania z kodu — od wersji EF6

Inny sposób, aby wygenerować widoków jest korzystanie z interfejsów API, który zapewnia EF. Przy użyciu tej metody należy swobody serializacji widoków, jednak lubisz, ale trzeba będzie również załadować widoków samodzielnie.

> [!NOTE]
> **EF6 począwszy tylko** — interfejsy API, przedstawione w tej sekcji zostały wprowadzone w programie Entity Framework 6. Jeśli używasz starszej wersji tych informacji nie ma zastosowania.

### <a name="generating-views"></a>Generowanie widoków

Interfejsy API, aby wygenerować widoki znajdują się w klasie System.Data.Entity.Core.Mapping.StorageMappingItemCollection. Aby pobrać StorageMappingCollection dla kontekstu, za pomocą obiektu MetadataWorkspace dla obiektu ObjectContext. Jeśli używasz nowszej interfejsu API typu DbContext, wówczas użytkownik może uzyskać dostępu do tego za pomocą IObjectContextAdapter, takich jak poniżej, w tym kodzie mamy wystąpienie usługi DbContext pochodnej wywołuje dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Po utworzeniu obiekt StorageMappingItemCollection można uzyskać dostęp do metod GenerateViews i ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

Pierwsza metoda tworzy słownik z wpis dla każdego widoku w mapowaniu kontenera. Druga metoda oblicza wartość skrótu dla mapowania jeden kontener i jest używany w czasie wykonywania do sprawdzania poprawności, model nie zmienił się od czasu widoki zostały wstępnie wygenerowane. Zastąpienia dwie metody są udostępniane dla złożonych scenariuszy obejmujących wiele mapowań kontenera.

Podczas generowania widoków spowoduje wywołanie metody GenerateViews i następnie zapisać wynikowy EntitySetBase i DbMappingView. Należy również przechowywać wyznaczania wartości skrótu, generowany przez metodę ComputeMappingHashValue.

### <a name="loading-views"></a>Trwa ładowanie widoków

Aby załadować widoki generowane przez metodę GenerateViews, możesz zapewnić EF klasę, która dziedziczy z klasy abstrakcyjnej DbMappingViewCache. DbMappingViewCache określa dwie metody, które należy zaimplementować:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Właściwość MappingHashValue musi zwracać wyznaczania wartości skrótu, generowany przez metodę ComputeMappingHashValue. Gdy EF jest przejście do zadania dla widoków najpierw wygeneruje i porównania wartości skrótu modelu przy użyciu skrótu zwracane przez tę właściwość. Jeśli nie są zgodne EF spowoduje zgłoszenie wyjątku EntityCommandCompilationException.

Metoda GetView będzie akceptować EntitySetBase i muszą zwracać DbMappingVIew, zawierający EntitySql, który został wygenerowany w tym został skojarzony z danym EntitySetBase w słowniku generowany przez metodę GenerateViews. Jeśli EF prosi o widoku, że nie masz następnie GetView powinna zwrócić wartość null.

Poniżej przedstawiono wyciąg z DbMappingViewCache, który jest generowany przy użyciu zaawansowanych narzędzi, jak opisano powyżej, w nim widać sposób przechowywania i pobierania EntitySql wymagane.

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

Mają zastosowanie do programów EF swoje DbMappingViewCache dodasz Użyj DbMappingViewCacheTypeAttribute, określając kontekst, który został utworzony dla. W poniższym kodzie za pomocą klasy MyMappingViewCache wnoszonym BlogContext.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

W przypadku bardziej złożonych scenariuszy wystąpienia pamięci podręcznej Widok mapowania można podać, określając fabrykę pamięci podręcznej Widok mapowania. Można to zrobić poprzez implementację klasy abstrakcyjnej System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. Wystąpienie fabryki pamięci podręcznej Widok mapowania, która jest używana można pobrać lub ustawić za pomocą StorageMappingItemCollection.MappingViewCacheFactoryproperty.
