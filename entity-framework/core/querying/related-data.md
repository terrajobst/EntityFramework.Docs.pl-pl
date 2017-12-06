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
# <a name="loading-related-data"></a>Trwa ładowanie powiązanych danych

Program Entity Framework Core umożliwia przy użyciu właściwości nawigacji w modelu załadować powiązanych jednostek. Istnieją trzy typowe wzorce O/RM używana do załadowania powiązanych danych.
* **Ładowanie wczesny** oznacza, że powiązane dane są ładowane z bazy danych w ramach początkowego zapytania.
* **Ładowanie jawne** oznacza, że powiązane jest jawnie załadować danych z bazy danych w późniejszym czasie.
* **Powolne ładowanie** oznacza, że pokrewne niewidocznie załadowaniu danych z bazy danych podczas uzyskiwania dostępu do właściwości nawigacji. Powolne ładowanie nie jest jeszcze możliwe EF podstawowych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="eager-loading"></a>Ładowanie wczesny

Można użyć `Include` metodę, aby określić powiązane dane mają zostać uwzględnione w wynikach zapytania. W poniższym przykładzie, blogów, z którego są zwracane w wynikach będzie miał ich `Posts` właściwości wypełniane przy użyciu powiązanych wpisów.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core będzie automatycznie konfigurowania właściwości nawigacji z innymi obiektami, które wcześniej zostały załadowane do wystąpienia kontekstu. Dlatego nawet wtedy, gdy wyraźnie nie zawierają danych dla właściwości nawigacji, właściwość nadal może być wypełniana w przypadku niektórych lub wszystkich powiązanych jednostek wcześniej załadowane.


Powiązane dane z wielu relacji można uwzględnić w jednym zapytaniu.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>W tym wiele poziomów

Można przejść do relacji z obejmują wiele poziomów powiązanych danych przy użyciu `ThenInclude` metody. Poniższy przykład ładuje wszystkie blogi, ich powiązane ogłoszeń i autor każdego post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Tworzenia łańcucha wielu wywołań `ThenInclude` aby kontynuować, tym więcej poziomy powiązanych danych.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Możesz łączyć wszystko to zostać uwzględnione w jednym zapytaniu powiązane dane z różnych poziomach i wielu elementów głównych.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Można dołączyć wielu powiązanych jednostek dla jednego z jednostkami, które ma być. Na przykład podczas wykonywania zapytania `Blog`s, obejmują `Posts` , a następnie chcesz dołączyć zarówno `Author` i `Tags` z `Posts`. Aby to zrobić, należy określić każdy obejmować ścieżki od katalogu głównego. Na przykład `Blog -> Posts -> Author` i `Blog -> Posts -> Tags`. Oznacza to, że otrzymasz nadmiarowe sprzężeń, w większości przypadków będzie skonsolidować EF sprzężeń podczas generowania SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Ignorowane obejmuje

Jeśli zmienisz zapytanie tak, aby nie będzie już zwracać wystąpień typów jednostek, które rozpoczął się zapytanie, operatorów include są ignorowane.

W poniższym przykładzie operatory dołączania są oparte na `Blog`, ale następnie `Select` operator służy do zmiany zwracanego typu anonimowego przez zapytanie. W takim przypadku operatory include nie obowiązują.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Domyślnie EF Core będzie rejestrować ostrzeżenie podczas obejmują operatory są ignorowane. Zobacz [rejestrowanie](../miscellaneous/logging.md) Aby uzyskać więcej informacji o wyświetlaniu dane wyjściowe rejestrowania. Można zmienić to zachowanie, gdy operator include jest ignorowana, aby zgłosić lub nie rób. Odbywa się podczas konfigurowania opcji kontekstu — zwykle w `DbContext.OnConfiguring`, lub `Startup.cs` Jeśli używasz platformy ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Ładowanie jawne

> [!NOTE]  
> Ta funkcja została wprowadzona w EF Core 1.1.

Można w sposób jawny załadować właściwości nawigacji za pośrednictwem `DbContext.Entry(...)` interfejsu API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Można również jawnie załadować właściwość nawigacji, wykonując oddzielne zapytania, które zwraca powiązanych jednostek. Jeśli jest włączone śledzenie zmian, podczas ładowania jednostki, EF Core zostanie automatycznie Ustaw właściwości nawigacji entitiy nowo załadowanych do odwoływania się do żadnych jednostek już załadowany, a następnie ustaw właściwości nawigacji jednostki już załadowany do odwoływania się do nowo załadowanych jednostki.

### <a name="querying-related-entities"></a>Wykonywanie zapytania powiązanych jednostek

Można również uzyskać kwerendy LINQ, która reprezentuje zawartość właściwości nawigacji.

Pozwala na wykonywanie czynności, takie jak uruchomienie operatora agregacji za pośrednictwem powiązanych jednostek bez ładowania ich do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Można również filtrować, które powiązanych jednostek są ładowane do pamięci.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>powolne ładowanie

Powolne ładowanie nie jest jeszcze obsługiwany przez EF rdzeń. Możesz wyświetlić [elementu opóźnionego ładowania na naszych zaległości](https://github.com/aspnet/EntityFramework/issues/3797) do śledzenia tej funkcji.

## <a name="related-data-and-serialization"></a>Powiązane dane i serializacja

Ponieważ właściwości nawigacji automatycznie konfigurowania będzie EF Core, można na końcu cykli na wykresie obiektu. Na przykład ładowanie blogu i powiązanych spowoduje wpisów w obiekcie blog, który odwołuje się do kolekcji wpisów. Każdy z tych stanowisk ma odwołanie do blogu.

Niektórych struktur serializacji nie zezwalają na takie cykle. Na przykład struktury Json.NET zgłosi następujący wyjątek, jeśli encoutered cyklu.

> Newtonsoft.Json.JsonSerializationException: Self odwołujące się do pętli wykryto dla właściwości "Blog" z typem "MyApplication.Models.Blog".

Jeśli używasz platformy ASP.NET Core, można skonfigurować struktury Json.NET do ignorowania cykle znalezionych w grafie obiektu. Jest to wykonywane w `ConfigureServices(...)` metoda `Startup.cs`.

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
