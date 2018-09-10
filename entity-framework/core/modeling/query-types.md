---
title: Typy zapytań — EF Core
author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 54d960e2e2236e2d4185dedc48f51035f5c10e93
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250728"
---
# <a name="query-types"></a><span data-ttu-id="65867-102">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="65867-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="65867-103">Ta funkcja jest nowa w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="65867-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="65867-104">Oprócz typów jednostek, mogą zawierać model programu EF Core _typy zapytań_, który może służyć do przeprowadzania zapytań bazy danych dla danych, które nie są mapowane na typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="65867-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="65867-105">Porównanie typów zapytań do typów jednostek</span><span class="sxs-lookup"><span data-stu-id="65867-105">Compare query types to entity types</span></span>

<span data-ttu-id="65867-106">Typy zapytań są podobne do typów jednostek, w tym ich:</span><span class="sxs-lookup"><span data-stu-id="65867-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="65867-107">Można dodać do modelu albo w `OnModelCreating` lub za pośrednictwem "set" właściwości pochodnej _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="65867-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="65867-108">Obsługuje wiele tych samych funkcji mapowania, takich jak właściwości mapowania i nawigację dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="65867-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="65867-109">W sklepach relacyjnych można skonfigurować obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="65867-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="65867-110">Jednak różnią się one od jednostki typy w tym ich:</span><span class="sxs-lookup"><span data-stu-id="65867-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="65867-111">Nie wymagają klucza do zdefiniowania.</span><span class="sxs-lookup"><span data-stu-id="65867-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="65867-112">Nigdy nie są śledzone zmiany na _DbContext_ i w związku z tym są nigdy nie wstawione, zaktualizowane lub usunięte w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="65867-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="65867-113">Nigdy nie są odnajdywane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="65867-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="65867-114">Tylko obsługują podzbiór funkcji map nawigacji — w szczególności:</span><span class="sxs-lookup"><span data-stu-id="65867-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="65867-115">Nigdy nie mogą one działać jako główny koniec relacji.</span><span class="sxs-lookup"><span data-stu-id="65867-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="65867-116">Może zawierać tylko właściwości nawigacji odwołania, wskazując jednostek.</span><span class="sxs-lookup"><span data-stu-id="65867-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="65867-117">Jednostki nie może zawierać właściwości nawigacji, aby typy zapytań.</span><span class="sxs-lookup"><span data-stu-id="65867-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="65867-118">Szybkiego reagowania na _element ModelBuilder_ przy użyciu `Query` metody zamiast `Entity` metody.</span><span class="sxs-lookup"><span data-stu-id="65867-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="65867-119">Są mapowane na _DbContext_ za pośrednictwem właściwości typu `DbQuery<T>` zamiast `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="65867-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="65867-120">Są mapowane na obiekty bazy danych przy użyciu `ToView` metody, zamiast `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="65867-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="65867-121">Mogą być mapowane na _kwerendy_ — Definiowanie zapytanie jest zapytaniem dodatkowej zadeklarowanych w modelu, który działa źródła danych dla typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="65867-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="65867-122">Scenariusze użytkowania</span><span class="sxs-lookup"><span data-stu-id="65867-122">Usage scenarios</span></span>

<span data-ttu-id="65867-123">Niektóre scenariusze użycia główne typy zapytań to:</span><span class="sxs-lookup"><span data-stu-id="65867-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="65867-124">Służy jako typ zwracany dla ad hoc `FromSql()` zapytania.</span><span class="sxs-lookup"><span data-stu-id="65867-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="65867-125">Mapowanie na widoki baz danych.</span><span class="sxs-lookup"><span data-stu-id="65867-125">Mapping to database views.</span></span>
- <span data-ttu-id="65867-126">Mapowania tabel, które nie mają zdefiniowany klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="65867-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="65867-127">Mapowanie do zapytań zdefiniowanych w modelu.</span><span class="sxs-lookup"><span data-stu-id="65867-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="65867-128">Mapowanie na obiekty bazy danych</span><span class="sxs-lookup"><span data-stu-id="65867-128">Mapping to database objects</span></span>

<span data-ttu-id="65867-129">Mapowanie typu zapytania do obiektu bazy danych jest osiągane przy użyciu `ToView` wygodnego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="65867-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="65867-130">Z perspektywy programu EF Core jest podany w tej metodzie obiekt bazy danych _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, wstawiania lub operacje usuwania.</span><span class="sxs-lookup"><span data-stu-id="65867-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="65867-131">Jednak oznacza to, że obiekt bazy danych jest faktycznie wymagana do widoku bazy danych — może też być tabeli bazy danych, które będą traktowane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="65867-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="65867-132">Z drugiej strony, dla typów jednostek programu EF Core przyjęto założenie, że obiektu bazy danych określony w `ToTable` metoda może być traktowana jako _tabeli_, co oznacza, że mogą być używane jako źródło zapytania, ale wskazywane przez aktualizację, usuwanie i wstawianie operacje.</span><span class="sxs-lookup"><span data-stu-id="65867-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="65867-133">W rzeczywistości, można określić nazwy widoku bazy danych w `ToTable` i wszystko powinno działać prawidłowo tak długo, jak widok jest skonfigurowany jako nadaje się do aktualizacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="65867-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="65867-134">Przykład</span><span class="sxs-lookup"><span data-stu-id="65867-134">Example</span></span>

<span data-ttu-id="65867-135">Poniższy przykład pokazuje, jak zapytania widoku bazy danych za pomocą typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="65867-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="65867-136">[Przykład](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="65867-136">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="65867-137">Najpierw należy zdefiniować prosty model blogu i Post:</span><span class="sxs-lookup"><span data-stu-id="65867-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="65867-138">Następnie zdefiniuj widok prostej bazy danych, który umożliwi nam zapytania liczby stanowisk skojarzonych z każdym blog:</span><span class="sxs-lookup"><span data-stu-id="65867-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="65867-139">Następnie zdefiniuj klasę zawierającą wyniki z widoku bazy danych:</span><span class="sxs-lookup"><span data-stu-id="65867-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="65867-140">Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu `modelBuilder.Query<T>` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="65867-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="65867-141">Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:</span><span class="sxs-lookup"><span data-stu-id="65867-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="65867-142">Na koniec mamy zapytania widoku bazy danych w sposób standardowy:</span><span class="sxs-lookup"><span data-stu-id="65867-142">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="65867-143">Należy zauważyć, że również zdefiniowaliśmy właściwości zapytania poziom kontekstu (DbQuery) do działania jako główne zapytań dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="65867-143">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
