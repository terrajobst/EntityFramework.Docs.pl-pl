---
title: Zapytanie globalne filtry - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054761"
---
# <a name="global-query-filters"></a>Zapytanie globalne filtry

Zapytanie globalne filtry są predykaty zapytań LINQ (wyrażenia logicznego zwykle przekazany do LINQ *gdzie* — operator zapytań) stosowany do typów jednostek w modelu metadanych (zazwyczaj w *OnModelCreating*). Takie filtry są automatycznie stosowane do wszelkich zapytań LINQ tych typów jednostek, w tym odwołań do właściwości nawigacji do którego istnieje odwołanie pośrednie, takie jak przy użyciu Include lub bezpośredniego typów jednostek. Niektóre typowe zastosowania tej funkcji są następujące:

* **Usuń soft** — definiuje typ jednostki *IsDeleted* właściwości.
* **Wielodostępność** — definiuje typ jednostki *TenantId* właściwości.

## <a name="example"></a>Przykład

Poniższy przykład przedstawia użycie filtry globalne zapytania do zaimplementowania zachowania zapytania soft usunięcie i obsługi wielu dzierżawców w modelu prostego obsługi blogów.

> [!TIP]
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) w witrynie GitHub.

Najpierw należy zdefiniować jednostek:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Należy zwrócić uwagę deklaracji __tenantId_ na _blogu_ jednostki. To będzie można użyć do skojarzenia każdego wystąpienia blogu z określonych dzierżawy. Jest także zdefiniowana _IsDeleted_ właściwość _Post_ typu jednostki. Służy to na śledzenie czy _Post_ wystąpienie zostało "wszystkie usunięte nietrwale". Tj. Wystąpienie jest oznaczone jako usunięte withouth fizycznie usunięcie danych.

Skonfiguruj filtrów kwerendy w _OnModelCreating_ przy użyciu ```HasQueryFilter``` interfejsu API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Wyrażenie predykatu przekazany do _HasQueryFilter_ wywołania teraz zostaną automatycznie zastosowane do żadnych zapytań LINQ dla tych typów.

> [!TIP]
> Zwróć uwagę na użycie pola poziomu wystąpienia typu DbContext: ```_tenantId``` służy do określania bieżącej dzierżawie. Filtry na poziomie modelu użyje wartości z wystąpienia poprawny kontekst. Tj. Wystąpienie, które wykonuje zapytania.

## <a name="disabling-filters"></a>Wyłączanie filtrów

Filtry mogą być wyłączone dla poszczególnych zapytań LINQ za pomocą ```IgnoreQueryFilters()``` operatora.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Ograniczenia

Zapytanie globalne filtry mają następujące ograniczenia:

* Filtry nie może zawierać odwołań do właściwości nawigacji.
* Filtry można zdefiniować tylko dla elementu głównego typu jednostki z hierarchii dziedziczenia.
