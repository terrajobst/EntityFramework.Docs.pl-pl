---
title: Filtry zapytań globalnych - programu EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 0c7858d660665b4f17aedea2101452048f9aff25
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900296"
---
# <a name="global-query-filters"></a>Filtry zapytań globalnych

Filtry zapytań globalnych są predykatów zapytań LINQ (wyrażenia logicznego zwykle przekazywane do programu LINQ *gdzie* — operator zapytań) stosowane do typów jednostek w modelu metadanych (zwykle w *OnModelCreating*). Takie filtry są automatycznie stosowane do żadnych zapytań LINQ, obejmujące tych typów jednostek, w tym odwołania do właściwości nawigacji odwołanie pośrednio, takie jak przy użyciu Include lub bezpośredniego typów jednostek. Niektóre typowe aplikacje tej funkcji są następujące:

* **Usuwanie nietrwałe** — Określa typ jednostki *IsDeleted* właściwości.
* **Wielodostępność** — Określa typ jednostki *TenantId* właściwości.

## <a name="example"></a>Przykład

Poniższy przykład pokazuje sposób użycia globalne filtry kwerendy do implementacji zachowania kwerendy opcji soft-delete oraz obsługi wielu dzierżawców w modelu prostego do obsługi blogów.

> [!TIP]
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) w witrynie GitHub.

Najpierw należy zdefiniować jednostki:

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

Należy pamiętać, deklaracja __tenantId_ na _blogu_ jednostki. To posłuży do powiązania każde wystąpienie blogu z określonej dzierżawy. Jest również definiowany _IsDeleted_ właściwość _wpis_ typu jednostki. To jest używany ten element, aby śledzić, czy _wpis_ wystąpienie zostało "wszystkie usunięte nietrwale". Czyli Wystąpienie jest oznaczony jako usunięty withouth fizycznym usunięciu danych bazowych.

Następnie skonfiguruj filtrów zapytania w _OnModelCreating_ przy użyciu ```HasQueryFilter``` interfejsu API.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

Predykatu wyrażenie przekazany do _HasQueryFilter_ wywołania teraz zostaną automatycznie zastosowane na żadne zapytania LINQ dla tych typów.

> [!TIP]
> Zwróć uwagę na użycie pola poziomu wystąpienia typu DbContext: ```_tenantId``` używane do ustawiania bieżącej dzierżawy. Filtry na poziomie modelu będzie używać wartości z wystąpienia poprawnego kontekstu. Czyli Wystąpienie, które jest wykonywane zapytanie.

## <a name="disabling-filters"></a>Wyłączanie filtrów

Filtry mogą być wyłączone dla poszczególnych zapytań LINQ za pomocą ```IgnoreQueryFilters()``` operatora.

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Ograniczenia

Filtry zapytań globalnych mają następujące ograniczenia:

* Filtry nie może zawierać odwołania do właściwości nawigacji.
* Filtry można zdefiniować tylko dla głównego typu jednostki z hierarchii dziedziczenia.
