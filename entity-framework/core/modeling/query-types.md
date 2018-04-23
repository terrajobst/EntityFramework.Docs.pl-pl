---
title: Typy zapytań - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a><span data-ttu-id="cdf8d-102">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="cdf8d-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="cdf8d-103">Ta funkcja jest nowa w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cdf8d-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="cdf8d-104">Oprócz typy jednostek, model EF Core może zawierać _typy zapytań_, które mogą służyć do wykonywania kwerend bazy danych w odniesieniu do danych, który nie jest zamapowany na typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="cdf8d-105">Typy zapytań mają wiele podobieństw z typami jednostek:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="cdf8d-106">Ich można również dodać do modelu albo w `OnModelCreating`, lub za pośrednictwem właściwości "set" na pochodnym _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="cdf8d-107">Obsługuje wiele mapowanie funkcji, takich jak dziedziczenia mapowania właściwości nawigacji (zobacz ograniczenia poniżej), a relacyjne magazynów, możliwość konfigurowania obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="cdf8d-108">Jednak są one różne od podmiotu typy w tym ich:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="cdf8d-109">Nie wymagają klucza ma zostać zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="cdf8d-110">Nigdy nie są śledzone zmiany na _DbContext_ i w związku z tym nigdy nie wstawienia, zaktualizowane lub usunięte w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="cdf8d-111">Nigdy nie są wykrywane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="cdf8d-112">Obsługują tylko podzestaw możliwości mapowania nawigacji — w szczególności, nigdy nie mogą one działać jako główny koniec relacji.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-112">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="cdf8d-113">Są opisane na _element ModelBuilder_ przy użyciu `Query` metody zamiast `Entity` metody.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-113">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="cdf8d-114">Są mapowane na _DbContext_ za pośrednictwem właściwości typu `DbQuery<T>` zamiast `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="cdf8d-114">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="cdf8d-115">Są mapowane do obiektów bazy danych przy użyciu `ToView` metody, a nie `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-115">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="cdf8d-116">Mogą być mapowane na _kwerendy_ — Definiowanie zapytanie jest zapytaniem dodatkowej zadeklarowany w modelu, który działa źródła danych dla typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-116">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="cdf8d-117">Niektóre scenariusze użycia główne typy zapytań to:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-117">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="cdf8d-118">Służy jako typ zwracany dla ad hoc `FromSql()` zapytania.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-118">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="cdf8d-119">Mapowanie do widoków bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-119">Mapping to database views.</span></span>
- <span data-ttu-id="cdf8d-120">Mapowanie do tabel, które nie ma zdefiniowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-120">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="cdf8d-121">Mapowanie do zapytań, zdefiniowanego w modelu.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-121">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="cdf8d-122">Mapowanie typu zapytania do obiektu bazy danych jest osiągane przy użyciu `ToView` interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-122">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="cdf8d-123">Z perspektywy EF Core obiekt bazy danych określony w ramach tej metody jest _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, Wstaw lub usuwanie operacji.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-123">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="cdf8d-124">Jednak nie oznacza to, że obiektu bazy danych jest faktycznie musi być widoku bazy danych — mogą być alternatywnie tabeli bazy danych, które będą traktowane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-124">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="cdf8d-125">Z drugiej strony, dla typów jednostek EF Core przyjęto założenie, że obiekt bazy danych określona w `ToTable` metoda może być traktowana jako _tabeli_, co oznacza, że można użyć jako źródła zapytań, ale może wskazywane przez aktualizację, usuwanie i wstawianie operacje.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-125">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="cdf8d-126">W rzeczywistości należy określić nazwę widoku bazy danych w `ToTable` i wszystko powinny działać prawidłowo tak długo, jak widok jest skonfigurowany tak, aby nadaje się do aktualizacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-126">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="cdf8d-127">Przykład</span><span class="sxs-lookup"><span data-stu-id="cdf8d-127">Example</span></span>

<span data-ttu-id="cdf8d-128">Poniższy przykład przedstawia użycie typu zapytania zbadać widok bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-128">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="cdf8d-129">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-129">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="cdf8d-130">Najpierw należy zdefiniować modelu prostego blogu i Post:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-130">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="cdf8d-131">Następnie określ widok proste bazy danych, który pozwoli nam się zapytanie o liczbę wpisów skojarzone z każdym blogu:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-131">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="cdf8d-132">Następnie określ klasę do przechowywania wyników z widoku bazy danych:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-132">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="cdf8d-133">Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu `modelBuilder.Query<T>` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-133">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="cdf8d-134">Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-134">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="cdf8d-135">Na koniec mamy kwerendy widoku bazy danych w standardowy sposób:</span><span class="sxs-lookup"><span data-stu-id="cdf8d-135">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="cdf8d-136">Należy zauważyć, że firma Microsoft również zdefiniowanych właściwości poziomu zapytania kontekstu (DbQuery) do działania jako katalog główny dla zapytań dotyczących tego typu.</span><span class="sxs-lookup"><span data-stu-id="cdf8d-136">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
