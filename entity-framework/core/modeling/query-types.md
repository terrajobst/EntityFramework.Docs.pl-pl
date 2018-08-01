---
title: Typy zapytań — EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 5a2cd451da8833daf2c315419559eb4a2c705b13
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388484"
---
# <a name="query-types"></a><span data-ttu-id="ff486-102">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="ff486-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="ff486-103">Ta funkcja jest nowa w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="ff486-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="ff486-104">Oprócz typów jednostek, mogą zawierać model programu EF Core _typy zapytań_, który może służyć do przeprowadzania zapytań bazy danych dla danych, które nie są mapowane na typy jednostek.</span><span class="sxs-lookup"><span data-stu-id="ff486-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="ff486-105">Typy zapytań mają wiele podobieństw przy użyciu typów jednostek:</span><span class="sxs-lookup"><span data-stu-id="ff486-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="ff486-106">One mogą być również dodawane do modelu albo w `OnModelCreating`, lub za pośrednictwem "set" właściwości pochodnej _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="ff486-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="ff486-107">Obsługiwane są też wiele tych samych funkcji mapowania, takich jak dziedziczenie mapowania właściwości nawigacji (zobacz poniżej ograniczenia) i w sklepach relacyjnych, możliwość konfigurowania obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="ff486-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="ff486-108">Jednak różnią się one od jednostki typy w tym ich:</span><span class="sxs-lookup"><span data-stu-id="ff486-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="ff486-109">Nie wymagają klucza do zdefiniowania.</span><span class="sxs-lookup"><span data-stu-id="ff486-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="ff486-110">Nigdy nie są śledzone zmiany na _DbContext_ i w związku z tym są nigdy nie wstawione, zaktualizowane lub usunięte w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ff486-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="ff486-111">Nigdy nie są odnajdywane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="ff486-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="ff486-112">Tylko obsługują podzbiór funkcji map nawigacji — w szczególności:</span><span class="sxs-lookup"><span data-stu-id="ff486-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="ff486-113">Nigdy nie mogą one działać jako główny koniec relacji.</span><span class="sxs-lookup"><span data-stu-id="ff486-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="ff486-114">Może zawierać tylko właściwości nawigacji odwołania, wskazując jednostek.</span><span class="sxs-lookup"><span data-stu-id="ff486-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="ff486-115">Jednostki nie może zawierać właściwości nawigacji, aby typy zapytań.</span><span class="sxs-lookup"><span data-stu-id="ff486-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="ff486-116">Szybkiego reagowania na _element ModelBuilder_ przy użyciu `Query` metody zamiast `Entity` metody.</span><span class="sxs-lookup"><span data-stu-id="ff486-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="ff486-117">Są mapowane na _DbContext_ za pośrednictwem właściwości typu `DbQuery<T>` zamiast `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="ff486-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="ff486-118">Są mapowane na obiekty bazy danych przy użyciu `ToView` metody, zamiast `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="ff486-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="ff486-119">Mogą być mapowane na _kwerendy_ — Definiowanie zapytanie jest zapytaniem dodatkowej zadeklarowanych w modelu, który działa źródła danych dla typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ff486-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="ff486-120">Niektóre scenariusze użycia główne typy zapytań to:</span><span class="sxs-lookup"><span data-stu-id="ff486-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="ff486-121">Służy jako typ zwracany dla ad hoc `FromSql()` zapytania.</span><span class="sxs-lookup"><span data-stu-id="ff486-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="ff486-122">Mapowanie na widoki baz danych.</span><span class="sxs-lookup"><span data-stu-id="ff486-122">Mapping to database views.</span></span>
- <span data-ttu-id="ff486-123">Mapowania tabel, które nie mają zdefiniowany klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="ff486-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="ff486-124">Mapowanie do zapytań zdefiniowanych w modelu.</span><span class="sxs-lookup"><span data-stu-id="ff486-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="ff486-125">Mapowanie typu zapytania do obiektu bazy danych jest osiągane przy użyciu `ToView` wygodnego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ff486-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="ff486-126">Z perspektywy programu EF Core jest podany w tej metodzie obiekt bazy danych _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, wstawiania lub operacje usuwania.</span><span class="sxs-lookup"><span data-stu-id="ff486-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="ff486-127">Jednak oznacza to, że obiekt bazy danych jest faktycznie wymagana do widoku bazy danych — może też być tabeli bazy danych, które będą traktowane jako tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="ff486-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="ff486-128">Z drugiej strony, dla typów jednostek programu EF Core przyjęto założenie, że obiektu bazy danych określony w `ToTable` metoda może być traktowana jako _tabeli_, co oznacza, że mogą być używane jako źródło zapytania, ale wskazywane przez aktualizację, usuwanie i wstawianie operacje.</span><span class="sxs-lookup"><span data-stu-id="ff486-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="ff486-129">W rzeczywistości, można określić nazwy widoku bazy danych w `ToTable` i wszystko powinno działać prawidłowo tak długo, jak widok jest skonfigurowany jako nadaje się do aktualizacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ff486-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="ff486-130">Przykład</span><span class="sxs-lookup"><span data-stu-id="ff486-130">Example</span></span>

<span data-ttu-id="ff486-131">Poniższy przykład pokazuje, jak zapytania widoku bazy danych za pomocą typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="ff486-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="ff486-132">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="ff486-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="ff486-133">Najpierw należy zdefiniować prosty model blogu i Post:</span><span class="sxs-lookup"><span data-stu-id="ff486-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="ff486-134">Następnie zdefiniuj widok prostej bazy danych, który umożliwi nam zapytania liczby stanowisk skojarzonych z każdym blog:</span><span class="sxs-lookup"><span data-stu-id="ff486-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="ff486-135">Następnie zdefiniuj klasę zawierającą wyniki z widoku bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ff486-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="ff486-136">Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu `modelBuilder.Query<T>` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ff486-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="ff486-137">Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:</span><span class="sxs-lookup"><span data-stu-id="ff486-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="ff486-138">Na koniec mamy zapytania widoku bazy danych w sposób standardowy:</span><span class="sxs-lookup"><span data-stu-id="ff486-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="ff486-139">Należy zauważyć, że również zdefiniowaliśmy właściwości zapytania poziom kontekstu (DbQuery) do działania jako główne zapytań dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="ff486-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
