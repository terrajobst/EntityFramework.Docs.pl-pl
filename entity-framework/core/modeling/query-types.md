---
title: Typy zapytań - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163177"
---
# <a name="query-types"></a><span data-ttu-id="8349e-102">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="8349e-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="8349e-103">Ta funkcja jest nowa w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="8349e-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="8349e-104">Oprócz typy jednostek, model EF Core może zawierać _typy zapytań_, które mogą służyć do wykonywania kwerend bazy danych w odniesieniu do danych, który nie jest zamapowany na typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="8349e-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="8349e-105">Typy zapytań mają wiele podobieństw z typami jednostek:</span><span class="sxs-lookup"><span data-stu-id="8349e-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="8349e-106">Ich można również dodać do modelu albo w `OnModelCreating`, lub za pośrednictwem właściwości "set" na pochodnym _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="8349e-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="8349e-107">Obsługuje wiele mapowanie funkcji, takich jak dziedziczenia mapowania właściwości nawigacji (zobacz ograniczenia poniżej), a relacyjne magazynów, możliwość konfigurowania obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="8349e-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="8349e-108">Jednak są one różne od podmiotu typy w tym ich:</span><span class="sxs-lookup"><span data-stu-id="8349e-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="8349e-109">Nie wymagają klucza ma zostać zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="8349e-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="8349e-110">Nigdy nie są śledzone zmiany na _DbContext_ i w związku z tym nigdy nie wstawienia, zaktualizowane lub usunięte w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8349e-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="8349e-111">Nigdy nie są wykrywane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="8349e-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="8349e-112">Tylko obsługuje podzestaw możliwości mapowania nawigacji — w szczególności:</span><span class="sxs-lookup"><span data-stu-id="8349e-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="8349e-113">Nigdy nie mogą one działać jako główny koniec relacji.</span><span class="sxs-lookup"><span data-stu-id="8349e-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="8349e-114">Może zawierać tylko właściwości nawigacji odwołania wskazujący jednostek.</span><span class="sxs-lookup"><span data-stu-id="8349e-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="8349e-115">Jednostki nie może zawierać właściwości nawigacji na typy zapytań.</span><span class="sxs-lookup"><span data-stu-id="8349e-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="8349e-116">Są opisane na _element ModelBuilder_ przy użyciu `Query` metody zamiast `Entity` metody.</span><span class="sxs-lookup"><span data-stu-id="8349e-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="8349e-117">Są mapowane na _DbContext_ za pośrednictwem właściwości typu `DbQuery<T>` zamiast `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="8349e-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="8349e-118">Są mapowane do obiektów bazy danych przy użyciu `ToView` metody, a nie `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="8349e-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="8349e-119">Mogą być mapowane na _kwerendy_ — Definiowanie zapytanie jest zapytaniem dodatkowej zadeklarowany w modelu, który działa źródła danych dla typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="8349e-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="8349e-120">Niektóre scenariusze użycia główne typy zapytań to:</span><span class="sxs-lookup"><span data-stu-id="8349e-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="8349e-121">Służy jako typ zwracany dla ad hoc `FromSql()` zapytania.</span><span class="sxs-lookup"><span data-stu-id="8349e-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="8349e-122">Mapowanie do widoków bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8349e-122">Mapping to database views.</span></span>
- <span data-ttu-id="8349e-123">Mapowanie do tabel, które nie ma zdefiniowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8349e-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="8349e-124">Mapowanie do zapytań, zdefiniowanego w modelu.</span><span class="sxs-lookup"><span data-stu-id="8349e-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="8349e-125">Mapowanie typu zapytania do obiektu bazy danych jest osiągane przy użyciu `ToView` interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="8349e-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="8349e-126">Z perspektywy EF Core obiekt bazy danych określony w ramach tej metody jest _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, Wstaw lub usuwanie operacji.</span><span class="sxs-lookup"><span data-stu-id="8349e-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="8349e-127">Jednak nie oznacza to, że obiektu bazy danych jest faktycznie musi być widoku bazy danych — mogą być alternatywnie tabeli bazy danych, które będą traktowane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="8349e-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="8349e-128">Z drugiej strony, dla typów jednostek EF Core przyjęto założenie, że obiekt bazy danych określona w `ToTable` metoda może być traktowana jako _tabeli_, co oznacza, że można użyć jako źródła zapytań, ale może wskazywane przez aktualizację, usuwanie i wstawianie operacje.</span><span class="sxs-lookup"><span data-stu-id="8349e-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="8349e-129">W rzeczywistości należy określić nazwę widoku bazy danych w `ToTable` i wszystko powinny działać prawidłowo tak długo, jak widok jest skonfigurowany tak, aby nadaje się do aktualizacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8349e-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="8349e-130">Przykład</span><span class="sxs-lookup"><span data-stu-id="8349e-130">Example</span></span>

<span data-ttu-id="8349e-131">Poniższy przykład przedstawia użycie typu zapytania zbadać widok bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8349e-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="8349e-132">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="8349e-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="8349e-133">Najpierw należy zdefiniować modelu prostego blogu i Post:</span><span class="sxs-lookup"><span data-stu-id="8349e-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="8349e-134">Następnie określ widok proste bazy danych, który pozwoli nam się zapytanie o liczbę wpisów skojarzone z każdym blogu:</span><span class="sxs-lookup"><span data-stu-id="8349e-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="8349e-135">Następnie określ klasę do przechowywania wyników z widoku bazy danych:</span><span class="sxs-lookup"><span data-stu-id="8349e-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="8349e-136">Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu `modelBuilder.Query<T>` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="8349e-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="8349e-137">Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:</span><span class="sxs-lookup"><span data-stu-id="8349e-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="8349e-138">Na koniec mamy kwerendy widoku bazy danych w standardowy sposób:</span><span class="sxs-lookup"><span data-stu-id="8349e-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="8349e-139">Należy zauważyć, że firma Microsoft również zdefiniowanych właściwości poziomu zapytania kontekstu (DbQuery) do działania jako katalog główny dla zapytań dotyczących tego typu.</span><span class="sxs-lookup"><span data-stu-id="8349e-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
