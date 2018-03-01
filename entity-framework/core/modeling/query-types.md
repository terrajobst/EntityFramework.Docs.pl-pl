---
title: "Typy zapytań - EF Core"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: d03c4b1d5635530e63b93e051cb69583718deb4e
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="query-types"></a><span data-ttu-id="adb8e-102">Typy zapytań</span><span class="sxs-lookup"><span data-stu-id="adb8e-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="adb8e-103">Ta funkcja jest nowa w programie EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="adb8e-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="adb8e-104">Typy zapytań są typy wyników zapytania tylko do odczytu do dodania do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="adb8e-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="adb8e-105">Typy zapytań włączenia zapytań ad hoc (np. typy anonimowe), ale są bardziej elastyczne, ponieważ mają one określona konfiguracja mapowania.</span><span class="sxs-lookup"><span data-stu-id="adb8e-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="adb8e-106">Są one podobny do typów jednostek w tym:</span><span class="sxs-lookup"><span data-stu-id="adb8e-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="adb8e-107">Są one typy obiektów POCO C#, które są dodawane do modelu w ```OnModelCreating``` przy użyciu ```ModelBuilder.Query``` metody, lub za pomocą właściwości DbContext "set" (dla zapytania typy taka właściwość jest typu ```DbQuery<T>``` zamiast który ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="adb8e-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather that ```DbSet<T>```).</span></span>
- <span data-ttu-id="adb8e-108">Obsługuje wiele funkcji mapowania jako typy jednostek regularne.</span><span class="sxs-lookup"><span data-stu-id="adb8e-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="adb8e-109">Na przykład mapowania dziedziczenia, nawigacji (zobacz poniżej limitiations) i w sklepach relacyjne, możliwość konfigurowania obiektów schematu docelowej bazy danych za pośrednictwem ```ToTable```, ```HasColumn``` metod fluent api (lub adnotacji danych).</span><span class="sxs-lookup"><span data-stu-id="adb8e-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="adb8e-110">Typy zapytań różnią się od podmiotu typy w tym ich:</span><span class="sxs-lookup"><span data-stu-id="adb8e-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="adb8e-111">Nie wymagają klucza ma zostać zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="adb8e-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="adb8e-112">Nigdy nie są śledzone przez obiekt śledzący zmiany.</span><span class="sxs-lookup"><span data-stu-id="adb8e-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="adb8e-113">Nigdy nie są wykrywane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="adb8e-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="adb8e-114">Obsługują tylko podzestaw możliwości mapowania nawigacji — w szczególności, nigdy nie mogą one działać jako główny koniec relacji.</span><span class="sxs-lookup"><span data-stu-id="adb8e-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="adb8e-115">Mogą być mapowane na _kwerendy_ -A Definiowanie zapytanie jest zapytaniem dodatkowej, będący źródłem danych dla typu zapytania.</span><span class="sxs-lookup"><span data-stu-id="adb8e-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="adb8e-116">Niektóre scenariusze użycia główne typy zapytań to:</span><span class="sxs-lookup"><span data-stu-id="adb8e-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="adb8e-117">Mapowanie do widoków bazy danych.</span><span class="sxs-lookup"><span data-stu-id="adb8e-117">Mapping to database views.</span></span>
- <span data-ttu-id="adb8e-118">Mapowanie do tabel, które nie ma zdefiniowanego klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="adb8e-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="adb8e-119">Służy jako typ zwracany dla ad hoc ```FromSql()``` zapytania.</span><span class="sxs-lookup"><span data-stu-id="adb8e-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="adb8e-120">Mapowanie do zapytań, zdefiniowanego w modelu.</span><span class="sxs-lookup"><span data-stu-id="adb8e-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="adb8e-121">Mapowanie typu zapytania do widoku bazy danych jest osiągane przy użyciu ```ToTable``` interfejsu API fluent.</span><span class="sxs-lookup"><span data-stu-id="adb8e-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="adb8e-122">Przykład</span><span class="sxs-lookup"><span data-stu-id="adb8e-122">Example</span></span>

<span data-ttu-id="adb8e-123">Poniższy przykład przedstawia użycie typu zapytania zbadać widok bazy danych.</span><span class="sxs-lookup"><span data-stu-id="adb8e-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="adb8e-124">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="adb8e-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="adb8e-125">Najpierw należy zdefiniować modelu prostego blogu i Post:</span><span class="sxs-lookup"><span data-stu-id="adb8e-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="adb8e-126">Następnie określ widok proste bazy danych, który pozwoli nam się zapytanie o liczbę wpisów skojarzone z każdym blogu:</span><span class="sxs-lookup"><span data-stu-id="adb8e-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="adb8e-127">Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu ```modelBuilder.Query<T>``` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="adb8e-127">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="adb8e-128">Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:</span><span class="sxs-lookup"><span data-stu-id="adb8e-128">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="adb8e-129">Na koniec mamy kwerendy widoku bazy danych w standardowy sposób:</span><span class="sxs-lookup"><span data-stu-id="adb8e-129">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="adb8e-130">Należy zauważyć, że firma Microsoft również zdefiniowanych właściwości poziomu zapytania kontekstu (DbQuery) do działania jako katalog główny dla zapytań dotyczących tego typu.</span><span class="sxs-lookup"><span data-stu-id="adb8e-130">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
