---
title: Jak działają zapytania — EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: ba0d68469530e6272ffbb51946d7856122a261c7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656240"
---
# <a name="how-queries-work"></a><span data-ttu-id="2b354-102">Jak działają zapytania</span><span class="sxs-lookup"><span data-stu-id="2b354-102">How Queries Work</span></span>

<span data-ttu-id="2b354-103">Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2b354-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="2b354-104">LINQ umożliwia pisanie kwerend o C# jednoznacznie określonym typie w oparciu o pochodne klasy kontekstowe i jednostki za pomocą (lub języka .NET).</span><span class="sxs-lookup"><span data-stu-id="2b354-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="2b354-105">Okres istnienia zapytania</span><span class="sxs-lookup"><span data-stu-id="2b354-105">The life of a query</span></span>

<span data-ttu-id="2b354-106">Poniżej znajduje się ogólny przegląd procesu każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="2b354-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="2b354-107">Zapytanie LINQ jest przetwarzane przez Entity Framework Core, aby utworzyć reprezentację, która jest gotowa do przetworzenia przez dostawcę bazy danych</span><span class="sxs-lookup"><span data-stu-id="2b354-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="2b354-108">Wynik jest buforowany w taki sposób, aby przetwarzanie tego procesu nie było wykonywane za każdym razem, gdy zapytanie jest wykonywane</span><span class="sxs-lookup"><span data-stu-id="2b354-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="2b354-109">Wynik jest przesyłany do dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="2b354-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="2b354-110">Dostawca bazy danych identyfikuje, które części zapytania można ocenić w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2b354-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="2b354-111">Te części zapytania są tłumaczone na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych)</span><span class="sxs-lookup"><span data-stu-id="2b354-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="2b354-112">Co najmniej jedno zapytanie jest wysyłane do bazy danych i zwrócony zestaw wyników (wyniki są wartościami z bazy danych, a nie wystąpień jednostek)</span><span class="sxs-lookup"><span data-stu-id="2b354-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="2b354-113">Dla każdego elementu w zestawie wyników</span><span class="sxs-lookup"><span data-stu-id="2b354-113">For each item in the result set</span></span>
   1. <span data-ttu-id="2b354-114">Jeśli jest to zapytanie śledzenia, program EF sprawdza, czy dane reprezentują jednostkę już w monitorze zmian dla wystąpienia kontekstu</span><span class="sxs-lookup"><span data-stu-id="2b354-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="2b354-115">Jeśli tak, zostanie zwrócona Istniejąca jednostka</span><span class="sxs-lookup"><span data-stu-id="2b354-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="2b354-116">Jeśli nie, zostanie utworzona nowa jednostka, śledzenie zmian jest skonfigurowane, a nowa jednostka zostanie zwrócona</span><span class="sxs-lookup"><span data-stu-id="2b354-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="2b354-117">Jeśli jest to zapytanie bez śledzenia, program Dr sprawdza, czy dane reprezentują jednostkę już w zestawie wyników dla tego zapytania</span><span class="sxs-lookup"><span data-stu-id="2b354-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="2b354-118">Jeśli tak, zostanie zwrócona Istniejąca jednostka <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="2b354-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="2b354-119">Jeśli nie, zostanie utworzona i zwrócona nowa jednostka</span><span class="sxs-lookup"><span data-stu-id="2b354-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="2b354-120"><sup>(1)</sup> zapytania bez śledzenia używają słabych odwołań do śledzenia jednostek, które zostały już zwrócone.</span><span class="sxs-lookup"><span data-stu-id="2b354-120"><sup>(1)</sup> No-tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="2b354-121">Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres i zostanie uruchomione odzyskiwanie pamięci, może zostać wyświetlone nowe wystąpienie jednostki.</span><span class="sxs-lookup"><span data-stu-id="2b354-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="2b354-122">Gdy zapytania są wykonywane</span><span class="sxs-lookup"><span data-stu-id="2b354-122">When queries are executed</span></span>

<span data-ttu-id="2b354-123">Gdy wywołujesz operatory LINQ, po prostu tworzysz reprezentację w pamięci zapytania.</span><span class="sxs-lookup"><span data-stu-id="2b354-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="2b354-124">Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy są używane wyniki.</span><span class="sxs-lookup"><span data-stu-id="2b354-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="2b354-125">Najczęstsze operacje, które powodują, że zapytanie wysyłane do bazy danych są następujące:</span><span class="sxs-lookup"><span data-stu-id="2b354-125">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="2b354-126">Iterowanie wyników w pętli `for`</span><span class="sxs-lookup"><span data-stu-id="2b354-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="2b354-127">Użycie operatora, takiego jak `ToList`, `ToArray`, `Single``Count`</span><span class="sxs-lookup"><span data-stu-id="2b354-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="2b354-128">Wiązanie danych z wyników zapytania do interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="2b354-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="2b354-129">**Zawsze Weryfikuj dane wejściowe użytkownika:** Chociaż EF Core chroni przed atakami polegającymi na iniekcji SQL przy użyciu parametrów i literałów ucieczki w zapytaniach, nie weryfikuje danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="2b354-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="2b354-130">Odpowiednie sprawdzenie na potrzeby aplikacji należy wykonać, zanim wartości z niezaufanych źródeł są używane w zapytaniach LINQ, przypisane do właściwości jednostki lub przekazane do innych EF Core interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="2b354-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="2b354-131">Obejmuje to wszelkie dane wejściowe użytkownika używane do dynamicznego konstruowania zapytań.</span><span class="sxs-lookup"><span data-stu-id="2b354-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="2b354-132">Nawet w przypadku korzystania z LINQ, jeśli akceptujesz dane wprowadzane przez użytkownika w wyrażeniach kompilacji, musisz upewnić się, że można skonstruować tylko zamierzone wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="2b354-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
