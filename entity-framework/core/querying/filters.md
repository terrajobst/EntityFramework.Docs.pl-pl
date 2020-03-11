---
title: Filtry zapytań globalnych - programu EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417730"
---
# <a name="global-query-filters"></a><span data-ttu-id="c482f-102">Filtry zapytań globalnych</span><span class="sxs-lookup"><span data-stu-id="c482f-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="c482f-103">Ta funkcja została wprowadzona w EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="c482f-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="c482f-104">Globalne filtry zapytań to predykaty zapytań LINQ (wyrażenie logiczne zwykle przenoszone do składnika LINQ *WHERE* operator zapytań) zastosowane do typów jednostek w modelu metadanych (zwykle w *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="c482f-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="c482f-105">Takie filtry są automatycznie stosowane do żadnych zapytań LINQ, obejmujące tych typów jednostek, w tym odwołania do właściwości nawigacji odwołanie pośrednio, takie jak przy użyciu Include lub bezpośredniego typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="c482f-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="c482f-106">Niektóre typowe aplikacje tej funkcji są następujące:</span><span class="sxs-lookup"><span data-stu-id="c482f-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="c482f-107">**Usuwanie nietrwałe** — typ jednostki definiuje Właściwość *IsDeleted* .</span><span class="sxs-lookup"><span data-stu-id="c482f-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="c482f-108">**Wielodostępność** — typ jednostki definiuje Właściwość *TenantId* .</span><span class="sxs-lookup"><span data-stu-id="c482f-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="c482f-109">Przykład</span><span class="sxs-lookup"><span data-stu-id="c482f-109">Example</span></span>

<span data-ttu-id="c482f-110">Poniższy przykład pokazuje sposób użycia globalne filtry kwerendy do implementacji zachowania kwerendy opcji soft-delete oraz obsługi wielu dzierżawców w modelu prostego do obsługi blogów.</span><span class="sxs-lookup"><span data-stu-id="c482f-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="c482f-111">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c482f-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="c482f-112">Najpierw należy zdefiniować jednostki:</span><span class="sxs-lookup"><span data-stu-id="c482f-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="c482f-113">Zanotuj deklarację pola _tenantId_ w jednostce _blogu_ .</span><span class="sxs-lookup"><span data-stu-id="c482f-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="c482f-114">To posłuży do powiązania każde wystąpienie blogu z określonej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="c482f-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="c482f-115">Zdefiniowana również jest właściwością _IsDeleted_ dla typu jednostki _post_ .</span><span class="sxs-lookup"><span data-stu-id="c482f-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="c482f-116">Służy do śledzenia, czy wystąpienie _post_ zostało usunięte z nietrwałego usuwania.</span><span class="sxs-lookup"><span data-stu-id="c482f-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="c482f-117">Oznacza to wystąpienie jest oznaczony jako usunięty bez fizycznym usunięciu danych bazowych.</span><span class="sxs-lookup"><span data-stu-id="c482f-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="c482f-118">Następnie skonfiguruj filtry zapytania w _OnModelCreating_ przy użyciu interfejsu API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="c482f-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="c482f-119">Wyrażenia predykatu przesłane do wywołań _HasQueryFilter_ będą teraz automatycznie stosowane do wszystkich zapytań LINQ dla tych typów.</span><span class="sxs-lookup"><span data-stu-id="c482f-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="c482f-120">Zwróć uwagę na użycie pola poziomu wystąpienia DbContext: `_tenantId` używany do ustawiania bieżącej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="c482f-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="c482f-121">Filtry na poziomie modelu będzie używać wartości z wystąpienia poprawny kontekst (oznacza to, że wystąpienie, które jest wykonywane zapytanie).</span><span class="sxs-lookup"><span data-stu-id="c482f-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="c482f-122">Obecnie nie jest możliwe zdefiniowanie wielu filtrów zapytania w tej samej jednostce — zostanie zastosowana tylko Ostatnia z nich.</span><span class="sxs-lookup"><span data-stu-id="c482f-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="c482f-123">Można jednak zdefiniować pojedynczy filtr z wieloma warunkami przy użyciu operatora logicznego _and_ ([`&&` in C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="c482f-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="c482f-124">Wyłączanie filtrów</span><span class="sxs-lookup"><span data-stu-id="c482f-124">Disabling Filters</span></span>

<span data-ttu-id="c482f-125">Filtry mogą być wyłączone dla poszczególnych zapytań LINQ przy użyciu operatora `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="c482f-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="c482f-126">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="c482f-126">Limitations</span></span>

<span data-ttu-id="c482f-127">Filtry zapytań globalnych mają następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="c482f-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="c482f-128">Filtry można zdefiniować tylko dla głównego typu jednostki z hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="c482f-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
