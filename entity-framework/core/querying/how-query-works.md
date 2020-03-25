---
title: Jak działają zapytania — EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136239"
---
# <a name="how-queries-work"></a><span data-ttu-id="bc9bb-102">Jak działają zapytania</span><span class="sxs-lookup"><span data-stu-id="bc9bb-102">How Queries Work</span></span>

<span data-ttu-id="bc9bb-103">Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="bc9bb-104">LINQ umożliwia pisanie kwerend o C# jednoznacznie określonym typie w oparciu o pochodne klasy kontekstowe i jednostki za pomocą (lub języka .NET).</span><span class="sxs-lookup"><span data-stu-id="bc9bb-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="bc9bb-105">Okres istnienia zapytania</span><span class="sxs-lookup"><span data-stu-id="bc9bb-105">The life of a query</span></span>

<span data-ttu-id="bc9bb-106">Poniżej znajduje się ogólny przegląd procesu każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="bc9bb-107">Zapytanie LINQ jest przetwarzane przez Entity Framework Core, aby utworzyć reprezentację, która jest gotowa do przetworzenia przez dostawcę bazy danych</span><span class="sxs-lookup"><span data-stu-id="bc9bb-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="bc9bb-108">Wynik jest buforowany w taki sposób, aby przetwarzanie tego procesu nie było wykonywane za każdym razem, gdy zapytanie jest wykonywane</span><span class="sxs-lookup"><span data-stu-id="bc9bb-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="bc9bb-109">Wynik jest przesyłany do dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="bc9bb-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="bc9bb-110">Dostawca bazy danych identyfikuje, które części zapytania można ocenić w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="bc9bb-111">Te części zapytania są tłumaczone na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych)</span><span class="sxs-lookup"><span data-stu-id="bc9bb-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="bc9bb-112">Zapytanie jest wysyłane do bazy danych i zwróconego zestawu wyników (wyniki są wartościami z bazy danych, a nie wystąpieniami jednostek)</span><span class="sxs-lookup"><span data-stu-id="bc9bb-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="bc9bb-113">Dla każdego elementu w zestawie wyników</span><span class="sxs-lookup"><span data-stu-id="bc9bb-113">For each item in the result set</span></span>
   1. <span data-ttu-id="bc9bb-114">Jeśli jest to zapytanie śledzenia, program EF sprawdza, czy dane reprezentują jednostkę już w monitorze zmian dla wystąpienia kontekstu</span><span class="sxs-lookup"><span data-stu-id="bc9bb-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="bc9bb-115">Jeśli tak, zostanie zwrócona Istniejąca jednostka</span><span class="sxs-lookup"><span data-stu-id="bc9bb-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="bc9bb-116">Jeśli nie, zostanie utworzona nowa jednostka, śledzenie zmian jest skonfigurowane, a nowa jednostka zostanie zwrócona</span><span class="sxs-lookup"><span data-stu-id="bc9bb-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="bc9bb-117">Jeśli to nie jest zapytanie śledzenia, Nowa jednostka jest zawsze tworzona i zwracana.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="bc9bb-118">Gdy zapytania są wykonywane</span><span class="sxs-lookup"><span data-stu-id="bc9bb-118">When queries are executed</span></span>

<span data-ttu-id="bc9bb-119">Gdy wywołujesz operatory LINQ, po prostu tworzysz reprezentację w pamięci zapytania.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="bc9bb-120">Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy są używane wyniki.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="bc9bb-121">Najczęstsze operacje, które powodują, że zapytanie wysyłane do bazy danych są następujące:</span><span class="sxs-lookup"><span data-stu-id="bc9bb-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="bc9bb-122">Iterowanie wyników w pętli `for`</span><span class="sxs-lookup"><span data-stu-id="bc9bb-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="bc9bb-123">Używanie operatora, takiego jak `ToList`, `ToArray`, `Single``Count` lub równoważne przeciążenia asynchroniczne</span><span class="sxs-lookup"><span data-stu-id="bc9bb-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="bc9bb-124">**Zawsze Weryfikuj dane wejściowe użytkownika:** Chociaż EF Core chroni przed atakami polegającymi na iniekcji SQL przy użyciu parametrów i literałów ucieczki w zapytaniach, nie weryfikuje danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="bc9bb-125">Odpowiednie sprawdzenie na potrzeby aplikacji należy wykonać, zanim wartości z niezaufanych źródeł są używane w zapytaniach LINQ, przypisane do właściwości jednostki lub przekazane do innych EF Core interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="bc9bb-126">Obejmuje to wszelkie dane wejściowe użytkownika używane do dynamicznego konstruowania zapytań.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="bc9bb-127">Nawet w przypadku korzystania z LINQ, jeśli akceptujesz dane wprowadzane przez użytkownika w wyrażeniach kompilacji, musisz upewnić się, że można skonstruować tylko zamierzone wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="bc9bb-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
