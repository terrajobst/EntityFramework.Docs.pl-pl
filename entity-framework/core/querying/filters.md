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
# <a name="global-query-filters"></a><span data-ttu-id="d83e5-102">Filtry zapytań globalnych</span><span class="sxs-lookup"><span data-stu-id="d83e5-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="d83e5-103">Ta funkcja została wprowadzona w EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d83e5-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="d83e5-104">Globalne filtry zapytań to predykaty kwerendy LINQ (wyrażenie logiczne zwykle przekazywane do operatora LINQ *Gdzie* kwerendy) stosowane do typów jednostek w modelu metadanych (zwykle w *Programie OnModelCreating).*</span><span class="sxs-lookup"><span data-stu-id="d83e5-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="d83e5-105">Takie filtry są automatycznie stosowane do wszystkich zapytań LINQ dotyczących tych typów jednostek, w tym typy jednostek, do których odwołuje się pośrednio, na przykład za pomocą funkcji Uwzględnij lub bezpośrednie odwołania do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d83e5-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="d83e5-106">Niektóre typowe zastosowania tej funkcji to:</span><span class="sxs-lookup"><span data-stu-id="d83e5-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="d83e5-107">**Usuwanie** nietrwałe — typ jednostki definiuje właściwość *IsDeleted.*</span><span class="sxs-lookup"><span data-stu-id="d83e5-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="d83e5-108">**Multi-dzierżawy** — typ jednostki definiuje *TenantId* właściwości.</span><span class="sxs-lookup"><span data-stu-id="d83e5-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="d83e5-109">Przykład</span><span class="sxs-lookup"><span data-stu-id="d83e5-109">Example</span></span>

<span data-ttu-id="d83e5-110">W poniższym przykładzie pokazano, jak za pomocą globalnych filtrów zapytań do implementowania zachowań kwerend typu "usuwanie nietrwałe" i wielonajemniczych w prostym modelu blogów.</span><span class="sxs-lookup"><span data-stu-id="d83e5-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="d83e5-111">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="d83e5-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="d83e5-112">Najpierw zdefiniuj jednostki:</span><span class="sxs-lookup"><span data-stu-id="d83e5-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="d83e5-113">Zanotuj deklarację _tenantId_ pola w _blogu_ jednostki.</span><span class="sxs-lookup"><span data-stu-id="d83e5-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="d83e5-114">Będzie to używane do skojarzenia każdego wystąpienia bloga z określonym dzierżawą.</span><span class="sxs-lookup"><span data-stu-id="d83e5-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="d83e5-115">Zdefiniowano również właściwość _IsDeleted_ w typie encji _Księguj._</span><span class="sxs-lookup"><span data-stu-id="d83e5-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="d83e5-116">Służy do śledzenia, czy _wystąpienie post_ zostało "usunięte bez śmigieł".</span><span class="sxs-lookup"><span data-stu-id="d83e5-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="d83e5-117">Oznacza to, że wystąpienie jest oznaczone jako usunięte bez fizycznego usuwania danych źródłowych.</span><span class="sxs-lookup"><span data-stu-id="d83e5-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="d83e5-118">Następnie skonfiguruj filtry zapytań w `HasQueryFilter` _Programie OnModelCreating_ przy użyciu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d83e5-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="d83e5-119">Wyrażenia predykatu przekazywane do wywołań _HasQueryFilter_ będą teraz automatycznie stosowane do wszystkich zapytań LINQ dla tych typów.</span><span class="sxs-lookup"><span data-stu-id="d83e5-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="d83e5-120">Należy zwrócić uwagę na użycie DbContext pole poziomu wystąpienia: `_tenantId` używane do ustawiania bieżącej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="d83e5-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="d83e5-121">Filtry na poziomie modelu użyją wartości z poprawnego wystąpienia kontekstu (czyli wystąpienia, które wykonuje kwerendę).</span><span class="sxs-lookup"><span data-stu-id="d83e5-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="d83e5-122">Obecnie nie jest możliwe zdefiniowanie wielu filtrów zapytań w tej samej jednostce — zostanie zastosowana tylko ostatnia.</span><span class="sxs-lookup"><span data-stu-id="d83e5-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="d83e5-123">Można jednak zdefiniować pojedynczy filtr z wieloma warunkami przy użyciu operatora logicznego _AND_ [ `&&` (w języku C#).](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)</span><span class="sxs-lookup"><span data-stu-id="d83e5-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="d83e5-124">Wyłączanie filtrów</span><span class="sxs-lookup"><span data-stu-id="d83e5-124">Disabling Filters</span></span>

<span data-ttu-id="d83e5-125">Filtry mogą być wyłączone dla poszczególnych `IgnoreQueryFilters()` zapytań LINQ przy użyciu operatora.</span><span class="sxs-lookup"><span data-stu-id="d83e5-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="d83e5-126">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="d83e5-126">Limitations</span></span>

<span data-ttu-id="d83e5-127">Globalne filtry zapytań mają następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="d83e5-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="d83e5-128">Filtry można zdefiniować tylko dla głównego typu jednostki hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="d83e5-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
