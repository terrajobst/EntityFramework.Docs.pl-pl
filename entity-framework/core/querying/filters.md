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
---
# <a name="global-query-filters"></a><span data-ttu-id="5e8b3-102">Zapytanie globalne filtry</span><span class="sxs-lookup"><span data-stu-id="5e8b3-102">Global Query Filters</span></span>

<span data-ttu-id="5e8b3-103">Zapytanie globalne filtry są predykaty zapytań LINQ (wyrażenia logicznego zwykle przekazany do LINQ *gdzie* — operator zapytań) stosowany do typów jednostek w modelu metadanych (zazwyczaj w *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="5e8b3-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="5e8b3-104">Takie filtry są automatycznie stosowane do wszelkich zapytań LINQ tych typów jednostek, w tym odwołań do właściwości nawigacji do którego istnieje odwołanie pośrednie, takie jak przy użyciu Include lub bezpośredniego typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="5e8b3-105">Niektóre typowe zastosowania tej funkcji są następujące:</span><span class="sxs-lookup"><span data-stu-id="5e8b3-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="5e8b3-106">**Usuń soft** — definiuje typ jednostki *IsDeleted* właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="5e8b3-107">**Wielodostępność** — definiuje typ jednostki *TenantId* właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="5e8b3-108">Przykład</span><span class="sxs-lookup"><span data-stu-id="5e8b3-108">Example</span></span>

<span data-ttu-id="5e8b3-109">Poniższy przykład przedstawia użycie filtry globalne zapytania do zaimplementowania zachowania zapytania soft usunięcie i obsługi wielu dzierżawców w modelu prostego obsługi blogów.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="5e8b3-110">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="5e8b3-111">Najpierw należy zdefiniować jednostek:</span><span class="sxs-lookup"><span data-stu-id="5e8b3-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="5e8b3-112">Należy zwrócić uwagę deklaracji __tenantId_ na _blogu_ jednostki.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="5e8b3-113">To będzie można użyć do skojarzenia każdego wystąpienia blogu z określonych dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="5e8b3-114">Jest także zdefiniowana _IsDeleted_ właściwość _Post_ typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="5e8b3-115">Służy to na śledzenie czy _Post_ wystąpienie zostało "wszystkie usunięte nietrwale".</span><span class="sxs-lookup"><span data-stu-id="5e8b3-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="5e8b3-116">Tj. Wystąpienie jest oznaczone jako usunięte withouth fizycznie usunięcie danych.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="5e8b3-117">Skonfiguruj filtrów kwerendy w _OnModelCreating_ przy użyciu ```HasQueryFilter``` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="5e8b3-118">Wyrażenie predykatu przekazany do _HasQueryFilter_ wywołania teraz zostaną automatycznie zastosowane do żadnych zapytań LINQ dla tych typów.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="5e8b3-119">Zwróć uwagę na użycie pola poziomu wystąpienia typu DbContext: ```_tenantId``` służy do określania bieżącej dzierżawie.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="5e8b3-120">Filtry na poziomie modelu użyje wartości z wystąpienia poprawny kontekst.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="5e8b3-121">Tj. Wystąpienie, które wykonuje zapytania.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="5e8b3-122">Wyłączanie filtrów</span><span class="sxs-lookup"><span data-stu-id="5e8b3-122">Disabling Filters</span></span>

<span data-ttu-id="5e8b3-123">Filtry mogą być wyłączone dla poszczególnych zapytań LINQ za pomocą ```IgnoreQueryFilters()``` operatora.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="5e8b3-124">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="5e8b3-124">Limitations</span></span>

<span data-ttu-id="5e8b3-125">Zapytanie globalne filtry mają następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="5e8b3-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="5e8b3-126">Filtry nie może zawierać odwołań do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="5e8b3-127">Filtry można zdefiniować tylko dla elementu głównego typu jednostki z hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="5e8b3-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
