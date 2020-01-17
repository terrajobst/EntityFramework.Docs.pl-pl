---
title: Filtry zapytań globalnych - programu EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f4ee9b77411290249e763f9cb8492eea61803e91
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124395"
---
# <a name="global-query-filters"></a>Filtry zapytań globalnych

> [!NOTE]
> Ta funkcja została wprowadzona w EF Core 2,0.

Filtry zapytań globalnych są predykatów zapytań LINQ (wyrażenia logicznego zwykle przekazywane do programu LINQ *gdzie* — operator zapytań) stosowane do typów jednostek w modelu metadanych (zwykle w *OnModelCreating*). Takie filtry są automatycznie stosowane do żadnych zapytań LINQ, obejmujące tych typów jednostek, w tym odwołania do właściwości nawigacji odwołanie pośrednio, takie jak przy użyciu Include lub bezpośredniego typów jednostek. Niektóre typowe aplikacje tej funkcji są następujące:

* **Usuwanie nietrwałe** — Określa typ jednostki *IsDeleted* właściwości.
* **Wielodostępność** — Określa typ jednostki *TenantId* właściwości.

## <a name="example"></a>Przykład

Poniższy przykład pokazuje sposób użycia globalne filtry kwerendy do implementacji zachowania kwerendy opcji soft-delete oraz obsługi wielu dzierżawców w modelu prostego do obsługi blogów.

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) użyty w tym artykule można zobaczyć w witrynie GitHub.

Najpierw należy zdefiniować jednostki:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Zanotuj deklarację pola _tenantId_ w jednostce _blogu_ . To posłuży do powiązania każde wystąpienie blogu z określonej dzierżawy. Jest również definiowany _IsDeleted_ właściwość _wpis_ typu jednostki. Służy to śledzenie czy _wpis_ wystąpienie zostało "wszystkie usunięte nietrwale". Oznacza to wystąpienie jest oznaczony jako usunięty bez fizycznym usunięciu danych bazowych.

Następnie skonfiguruj filtrów zapytania w _OnModelCreating_ przy użyciu `HasQueryFilter` interfejsu API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Predykatu wyrażenie przekazany do _HasQueryFilter_ wywołania teraz zostaną automatycznie zastosowane na żadne zapytania LINQ dla tych typów.

> [!TIP]
> Zwróć uwagę na użycie pola poziomu wystąpienia typu DbContext: `_tenantId` używane do ustawiania bieżącej dzierżawy. Filtry na poziomie modelu będzie używać wartości z wystąpienia poprawny kontekst (oznacza to, że wystąpienie, które jest wykonywane zapytanie).

> [!NOTE]
> Obecnie nie jest możliwe zdefiniowanie wielu filtrów zapytania w tej samej jednostce — zostanie zastosowana tylko Ostatnia z nich. Można jednak zdefiniować pojedynczy filtr z wieloma warunkami przy użyciu operatora logicznego _and_ ([`&&` in C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Wyłączanie filtrów

Filtry mogą być wyłączone dla poszczególnych zapytań LINQ za pomocą `IgnoreQueryFilters()` operatora.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Ograniczenia

Filtry zapytań globalnych mają następujące ograniczenia:

* Filtry można zdefiniować tylko dla głównego typu jednostki z hierarchii dziedziczenia.
