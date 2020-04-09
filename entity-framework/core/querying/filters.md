---
title: Globalne filtry zapytań — EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417730"
---
# <a name="global-query-filters"></a>Filtry zapytań globalnych

> [!NOTE]
> Ta funkcja została wprowadzona w EF Core 2.0.

Globalne filtry zapytań to predykaty kwerendy LINQ (wyrażenie logiczne zwykle przekazywane do operatora LINQ *Gdzie* kwerendy) stosowane do typów jednostek w modelu metadanych (zwykle w *Programie OnModelCreating).* Takie filtry są automatycznie stosowane do wszystkich zapytań LINQ dotyczących tych typów jednostek, w tym typy jednostek, do których odwołuje się pośrednio, na przykład za pomocą funkcji Uwzględnij lub bezpośrednie odwołania do właściwości nawigacji. Niektóre typowe zastosowania tej funkcji to:

* **Usuwanie** nietrwałe — typ jednostki definiuje właściwość *IsDeleted.*
* **Multi-dzierżawy** — typ jednostki definiuje *TenantId* właściwości.

## <a name="example"></a>Przykład

W poniższym przykładzie pokazano, jak za pomocą globalnych filtrów zapytań do implementowania zachowań kwerend typu "usuwanie nietrwałe" i wielonajemniczych w prostym modelu blogów.

> [!TIP]
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) przykład artykułu na GitHub.

Najpierw zdefiniuj jednostki:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Zanotuj deklarację _tenantId_ pola w _blogu_ jednostki. Będzie to używane do skojarzenia każdego wystąpienia bloga z określonym dzierżawą. Zdefiniowano również właściwość _IsDeleted_ w typie encji _Księguj._ Służy do śledzenia, czy _wystąpienie post_ zostało "usunięte bez śmigieł". Oznacza to, że wystąpienie jest oznaczone jako usunięte bez fizycznego usuwania danych źródłowych.

Następnie skonfiguruj filtry zapytań w `HasQueryFilter` _Programie OnModelCreating_ przy użyciu interfejsu API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Wyrażenia predykatu przekazywane do wywołań _HasQueryFilter_ będą teraz automatycznie stosowane do wszystkich zapytań LINQ dla tych typów.

> [!TIP]
> Należy zwrócić uwagę na użycie DbContext pole poziomu wystąpienia: `_tenantId` używane do ustawiania bieżącej dzierżawy. Filtry na poziomie modelu użyją wartości z poprawnego wystąpienia kontekstu (czyli wystąpienia, które wykonuje kwerendę).

> [!NOTE]
> Obecnie nie jest możliwe zdefiniowanie wielu filtrów zapytań w tej samej jednostce — zostanie zastosowana tylko ostatnia. Można jednak zdefiniować pojedynczy filtr z wieloma warunkami przy użyciu operatora logicznego _AND_ [ `&&` (w języku C#).](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)

## <a name="disabling-filters"></a>Wyłączanie filtrów

Filtry mogą być wyłączone dla poszczególnych `IgnoreQueryFilters()` zapytań LINQ przy użyciu operatora.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Ograniczenia

Globalne filtry zapytań mają następujące ograniczenia:

* Filtry można zdefiniować tylko dla głównego typu jednostki hierarchii dziedziczenia.
