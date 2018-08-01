---
title: Filtry zapytań globalnych - programu EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 6b7a4069917c93015a218c131ff0d0a3920fb69d
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388408"
---
# <a name="global-query-filters"></a><span data-ttu-id="d7647-102">Filtry zapytań globalnych</span><span class="sxs-lookup"><span data-stu-id="d7647-102">Global Query Filters</span></span>

<span data-ttu-id="d7647-103">Filtry zapytań globalnych są predykatów zapytań LINQ (wyrażenia logicznego zwykle przekazywane do programu LINQ *gdzie* — operator zapytań) stosowane do typów jednostek w modelu metadanych (zwykle w *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="d7647-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="d7647-104">Takie filtry są automatycznie stosowane do żadnych zapytań LINQ, obejmujące tych typów jednostek, w tym odwołania do właściwości nawigacji odwołanie pośrednio, takie jak przy użyciu Include lub bezpośredniego typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="d7647-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="d7647-105">Niektóre typowe aplikacje tej funkcji są następujące:</span><span class="sxs-lookup"><span data-stu-id="d7647-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="d7647-106">**Usuwanie nietrwałe** — Określa typ jednostki *IsDeleted* właściwości.</span><span class="sxs-lookup"><span data-stu-id="d7647-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="d7647-107">**Wielodostępność** — Określa typ jednostki *TenantId* właściwości.</span><span class="sxs-lookup"><span data-stu-id="d7647-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="d7647-108">Przykład</span><span class="sxs-lookup"><span data-stu-id="d7647-108">Example</span></span>

<span data-ttu-id="d7647-109">Poniższy przykład pokazuje sposób użycia globalne filtry kwerendy do implementacji zachowania kwerendy opcji soft-delete oraz obsługi wielu dzierżawców w modelu prostego do obsługi blogów.</span><span class="sxs-lookup"><span data-stu-id="d7647-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="d7647-110">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="d7647-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="d7647-111">Najpierw należy zdefiniować jednostki:</span><span class="sxs-lookup"><span data-stu-id="d7647-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="d7647-112">Należy pamiętać, deklaracja __tenantId_ na _blogu_ jednostki.</span><span class="sxs-lookup"><span data-stu-id="d7647-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="d7647-113">To posłuży do powiązania każde wystąpienie blogu z określonej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="d7647-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="d7647-114">Jest również definiowany _IsDeleted_ właściwość _wpis_ typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="d7647-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="d7647-115">Służy to śledzenie czy _wpis_ wystąpienie zostało "wszystkie usunięte nietrwale".</span><span class="sxs-lookup"><span data-stu-id="d7647-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="d7647-116">Oznacza to wystąpienie jest oznaczony jako usunięty bez fizycznym usunięciu danych bazowych.</span><span class="sxs-lookup"><span data-stu-id="d7647-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="d7647-117">Następnie skonfiguruj filtrów zapytania w _OnModelCreating_ przy użyciu ```HasQueryFilter``` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d7647-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="d7647-118">Predykatu wyrażenie przekazany do _HasQueryFilter_ wywołania teraz zostaną automatycznie zastosowane na żadne zapytania LINQ dla tych typów.</span><span class="sxs-lookup"><span data-stu-id="d7647-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="d7647-119">Zwróć uwagę na użycie pola poziomu wystąpienia typu DbContext: ```_tenantId``` używane do ustawiania bieżącej dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="d7647-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="d7647-120">Filtry na poziomie modelu będzie używać wartości z wystąpienia poprawny kontekst (oznacza to, że wystąpienie, które jest wykonywane zapytanie).</span><span class="sxs-lookup"><span data-stu-id="d7647-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="d7647-121">Wyłączanie filtrów</span><span class="sxs-lookup"><span data-stu-id="d7647-121">Disabling Filters</span></span>

<span data-ttu-id="d7647-122">Filtry mogą być wyłączone dla poszczególnych zapytań LINQ za pomocą ```IgnoreQueryFilters()``` operatora.</span><span class="sxs-lookup"><span data-stu-id="d7647-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="d7647-123">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="d7647-123">Limitations</span></span>

<span data-ttu-id="d7647-124">Filtry zapytań globalnych mają następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="d7647-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="d7647-125">Filtry nie może zawierać odwołania do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d7647-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="d7647-126">Filtry można zdefiniować tylko dla głównego typu jednostki z hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="d7647-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
