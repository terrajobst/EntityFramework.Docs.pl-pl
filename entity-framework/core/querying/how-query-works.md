---
title: Jak działają zapytania — EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136239"
---
# <a name="how-queries-work"></a><span data-ttu-id="29f6f-102">Jak działają kwerendy</span><span class="sxs-lookup"><span data-stu-id="29f6f-102">How Queries Work</span></span>

<span data-ttu-id="29f6f-103">Entity Framework Core używa języka zintegrowane zapytanie (LINQ) do kwerendy danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="29f6f-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="29f6f-104">LINQ umożliwia używanie języka C# (lub wybranego języka .NET) do pisania silnie wpisanych zapytań na podstawie kontekstu pochodnego i klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="29f6f-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="29f6f-105">Okres ważności kwerendy</span><span class="sxs-lookup"><span data-stu-id="29f6f-105">The life of a query</span></span>

<span data-ttu-id="29f6f-106">Poniżej przedstawiono omówienie wysokiego poziomu procesu, przez który przechodzi każda kwerenda.</span><span class="sxs-lookup"><span data-stu-id="29f6f-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="29f6f-107">Kwerenda LINQ jest przetwarzana przez entity framework core w celu utworzenia reprezentacji, która jest gotowa do przetworzenia przez dostawcę bazy danych</span><span class="sxs-lookup"><span data-stu-id="29f6f-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="29f6f-108">Wynik jest buforowany, dzięki czemu przetwarzanie to nie musi być wykonywane za każdym razem, gdy kwerenda jest wykonywana</span><span class="sxs-lookup"><span data-stu-id="29f6f-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="29f6f-109">Wynik jest przekazywany do dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="29f6f-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="29f6f-110">Dostawca bazy danych określa, które części kwerendy mogą być oceniane w bazie danych</span><span class="sxs-lookup"><span data-stu-id="29f6f-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="29f6f-111">Te części kwerendy są tłumaczone na język kwerend specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych)</span><span class="sxs-lookup"><span data-stu-id="29f6f-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="29f6f-112">Kwerenda jest wysyłana do bazy danych i zwracany zestaw wyników (wyniki są wartościami z bazy danych, a nie wystąpień encji)</span><span class="sxs-lookup"><span data-stu-id="29f6f-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="29f6f-113">Dla każdego elementu w zestawie wyników</span><span class="sxs-lookup"><span data-stu-id="29f6f-113">For each item in the result set</span></span>
   1. <span data-ttu-id="29f6f-114">Jeśli jest to kwerenda śledząca, EF sprawdza, czy dane reprezentują jednostkę już w monitorze zmian dla wystąpienia kontekstu</span><span class="sxs-lookup"><span data-stu-id="29f6f-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="29f6f-115">Jeśli tak, istniejąca jednostka jest zwracana</span><span class="sxs-lookup"><span data-stu-id="29f6f-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="29f6f-116">Jeśli nie, tworzona jest nowa encja, jest skonfigurowane śledzenie zmian, a nowa encja jest zwracana</span><span class="sxs-lookup"><span data-stu-id="29f6f-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="29f6f-117">Jeśli jest to kwerenda nie śledząca, nowa encja jest zawsze tworzona i zwracana</span><span class="sxs-lookup"><span data-stu-id="29f6f-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="29f6f-118">Podczas wykonywania zapytań</span><span class="sxs-lookup"><span data-stu-id="29f6f-118">When queries are executed</span></span>

<span data-ttu-id="29f6f-119">Podczas wywoływania operatorów LINQ, jesteś po prostu tworzenie reprezentacji w pamięci kwerendy.</span><span class="sxs-lookup"><span data-stu-id="29f6f-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="29f6f-120">Kwerenda jest wysyłana do bazy danych tylko wtedy, gdy wyniki są używane.</span><span class="sxs-lookup"><span data-stu-id="29f6f-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="29f6f-121">Najczęstsze operacje, które powodują kwerendę wysyłane do bazy danych są:</span><span class="sxs-lookup"><span data-stu-id="29f6f-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="29f6f-122">Iteracja wyników w `for` pętli</span><span class="sxs-lookup"><span data-stu-id="29f6f-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="29f6f-123">Korzystanie z operatora, `ToArray` `Single`takiego `Count` jak `ToList`, , lub równoważne przeciążenia asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="29f6f-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="29f6f-124">**Zawsze sprawdzaj poprawność danych wejściowych użytkownika:** Podczas EF Core chroni przed atakami iniekcji SQL przy użyciu parametrów i uciekania literały w kwerendach, nie sprawdza poprawności danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="29f6f-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="29f6f-125">Odpowiednie sprawdzanie poprawności, zgodnie z wymaganiami aplikacji, należy wykonać przed wartości z niezaufanych źródeł są używane w zapytaniach LINQ, przypisane do właściwości jednostki lub przekazywane do innych interfejsów API EF Core.</span><span class="sxs-lookup"><span data-stu-id="29f6f-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="29f6f-126">Obejmuje to wszelkie dane wejściowe użytkownika używane do dynamicznego konstruowania zapytań.</span><span class="sxs-lookup"><span data-stu-id="29f6f-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="29f6f-127">Nawet przy użyciu LINQ, jeśli akceptujesz dane wejściowe użytkownika do tworzenia wyrażeń, należy upewnić się, że tylko zamierzone wyrażenia mogą być konstruowane.</span><span class="sxs-lookup"><span data-stu-id="29f6f-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
