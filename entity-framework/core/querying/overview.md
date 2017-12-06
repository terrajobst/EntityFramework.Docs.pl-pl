---
title: Jak odpytuje Praca - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a><span data-ttu-id="845c4-102">Jak działają zapytań</span><span class="sxs-lookup"><span data-stu-id="845c4-102">How Queries Work</span></span>

<span data-ttu-id="845c4-103">Entity Framework Core używa języka integracji zapytania (LINQ) wykonać zapytania o dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="845c4-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="845c4-104">LINQ umożliwia przy użyciu języka C# (lub z języka .NET) do zapisania jednoznacznie zapytań opartych na klas pochodnych kontekstu i jednostki.</span><span class="sxs-lookup"><span data-stu-id="845c4-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="845c4-105">Czas życia zapytania</span><span class="sxs-lookup"><span data-stu-id="845c4-105">The life of a query</span></span>

<span data-ttu-id="845c4-106">Poniżej znajduje się wysokiego poziomu Omówienie procesu, który przechodzi każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="845c4-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="845c4-107">Zapytania LINQ jest przetwarzany przez Entity Framework Core do budowania reprezentacji, który jest gotowy do przetwarzania dostawca bazy danych</span><span class="sxs-lookup"><span data-stu-id="845c4-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="845c4-108">Wynik jest buforowany, dzięki czemu to przetwarzanie nie musi odbywać się za każdym razem, gdy zapytanie jest wykonywane</span><span class="sxs-lookup"><span data-stu-id="845c4-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="845c4-109">Wynik jest przekazywany do dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="845c4-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="845c4-110">Dostawca bazy danych identyfikuje części zapytania, które może przyjąć w bazie danych</span><span class="sxs-lookup"><span data-stu-id="845c4-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="845c4-111">Te części zapytania są tłumaczone na język określonej kwerendy bazy danych (np. SQL relacyjnej bazy danych)</span><span class="sxs-lookup"><span data-stu-id="845c4-111">These parts of the query are translated to database specific query language (e.g. SQL for a relational database)</span></span>
   3. <span data-ttu-id="845c4-112">Co najmniej jednego zapytania są wysyłane do bazy danych i zestaw zwrócony wyników (wyniki są wartości z bazy danych, nie wystąpień jednostek)</span><span class="sxs-lookup"><span data-stu-id="845c4-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="845c4-113">Dla każdego elementu w zestawie wyników</span><span class="sxs-lookup"><span data-stu-id="845c4-113">For each item in the result set</span></span>
   1. <span data-ttu-id="845c4-114">Jeśli jest to zapytanie śledzenia, EF sprawdza, czy dane reprezentuje jednostkę już w śledzenia zmian dla wystąpienia kontekstu</span><span class="sxs-lookup"><span data-stu-id="845c4-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="845c4-115">Jeśli tak, zwracany jest istniejącej jednostki</span><span class="sxs-lookup"><span data-stu-id="845c4-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="845c4-116">Jeśli nie jest tworzony nowy obiekt, skonfigurowano śledzenie zmian i nowy obiekt jest zwracany</span><span class="sxs-lookup"><span data-stu-id="845c4-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="845c4-117">Jeśli jest to zapytanie nie śledzenia, EF sprawdza, czy dane reprezentuje jednostkę już w zestawu wyników dla tego zapytania</span><span class="sxs-lookup"><span data-stu-id="845c4-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="845c4-118">Jeśli tak, zostanie zwrócony istniejącej jednostki <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="845c4-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="845c4-119">W przeciwnym razie nowy obiekt zostanie utworzona i zwrócona</span><span class="sxs-lookup"><span data-stu-id="845c4-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="845c4-120"><sup>(1) </sup> Nie ma zapytań, śledzenie użycia słabe odwołania do śledzenia jednostek, które już zostały zwrócone.</span><span class="sxs-lookup"><span data-stu-id="845c4-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="845c4-121">Jeśli poprzedni wynik o tej samej tożsamości wykracza poza zakres i wyrzucanie elementów bezużytecznych działa, możesz uzyskać nowego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="845c4-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="845c4-122">Podczas wykonywania kwerendy</span><span class="sxs-lookup"><span data-stu-id="845c4-122">When queries are executed</span></span>

<span data-ttu-id="845c4-123">Podczas wywoływania LINQ operatory są po prostu budowania reprezentacji w pamięci zapytania.</span><span class="sxs-lookup"><span data-stu-id="845c4-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="845c4-124">Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy wyniki są używane.</span><span class="sxs-lookup"><span data-stu-id="845c4-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="845c4-125">Najbardziej typowe operacje, które powoduje zapytania są wysyłane do bazy danych są:</span><span class="sxs-lookup"><span data-stu-id="845c4-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="845c4-126">Iteracja wyniki w `for` pętli</span><span class="sxs-lookup"><span data-stu-id="845c4-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="845c4-127">Przy użyciu operatora, takich jak `ToList`, `ToArray`, `Single`,`Count`</span><span class="sxs-lookup"><span data-stu-id="845c4-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="845c4-128">Element dataBinding wyników zapytania do interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="845c4-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="845c4-129">**Sprawdzanie poprawności danych wejściowych użytkownika:** podczas EF zapewnia ochronę przed atakami iniekcji kodu SQL, nie ma żadnych ogólne sprawdzania poprawności danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="845c4-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="845c4-130">W związku z tym jeśli wartości były przekazywane do interfejsów API, które są używane w zapytaniach LINQ, przypisane do właściwości obiektu itp., pochodzi z niezaufanego źródła, a następnie odpowiednią sprawdzania poprawności, zgodnie z wymaganiami aplikacji należy wykonać.</span><span class="sxs-lookup"><span data-stu-id="845c4-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="845c4-131">W tym wszystkie dane wejściowe użytkownika używane do dynamicznego tworzenia zapytania.</span><span class="sxs-lookup"><span data-stu-id="845c4-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="845c4-132">Nawet w przypadku używania LINQ, jeśli akceptujesz wprowadzenia danych przez użytkownika do kompilacji wyrażeń, które należy się upewnić, niż tylko danego wyrażenia może być skonstruowany.</span><span class="sxs-lookup"><span data-stu-id="845c4-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
