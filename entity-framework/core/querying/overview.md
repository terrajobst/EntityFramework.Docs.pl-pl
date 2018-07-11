---
title: Jak zapytania praca - programu EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 1d28d215302625cf2b6788359527a93a77b7e9fd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949387"
---
# <a name="how-queries-work"></a><span data-ttu-id="0190a-102">Jak działają zapytań</span><span class="sxs-lookup"><span data-stu-id="0190a-102">How Queries Work</span></span>

<span data-ttu-id="0190a-103">Entity Framework Core używa Language Integrated Query (LINQ) do zapytania o dane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0190a-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="0190a-104">LINQ umożliwia przy użyciu języka C# (lub wybranym języku .NET) do pisania zapytań silnie typizowaną oparte na klasach pochodnych kontekstu i jednostki.</span><span class="sxs-lookup"><span data-stu-id="0190a-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="0190a-105">Czas życia zapytania</span><span class="sxs-lookup"><span data-stu-id="0190a-105">The life of a query</span></span>

<span data-ttu-id="0190a-106">Poniżej znajduje się przegląd wysokiego poziomu procesu, który przechodzi każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="0190a-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="0190a-107">Zapytania LINQ są przetwarzane przez program Entity Framework Core do budowania reprezentacji, które jest gotowe do przetworzenia przez dostawcę bazy danych</span><span class="sxs-lookup"><span data-stu-id="0190a-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="0190a-108">Wynik jest buforowany, dzięki czemu to przetwarzanie nie musi odbywać się za każdym razem, gdy zapytanie jest wykonywane</span><span class="sxs-lookup"><span data-stu-id="0190a-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="0190a-109">Wynik jest przekazywany do dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="0190a-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="0190a-110">Dostawca bazy danych identyfikuje części zapytania, które mogą być obliczane w bazie danych</span><span class="sxs-lookup"><span data-stu-id="0190a-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="0190a-111">Te części zapytania są tłumaczone na języka użytego zapytania bazy danych (na przykład SQL dla relacyjnej bazy danych)</span><span class="sxs-lookup"><span data-stu-id="0190a-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="0190a-112">Co najmniej jeden zapytania są wysyłane do bazy danych i zwróconym zestawie wyników (wyniki są wartości z bazy danych, a nie wystąpień jednostek)</span><span class="sxs-lookup"><span data-stu-id="0190a-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="0190a-113">Dla każdego elementu w zestawie wyników</span><span class="sxs-lookup"><span data-stu-id="0190a-113">For each item in the result set</span></span>
   1. <span data-ttu-id="0190a-114">Jeśli jest to zapytania śledzenia, EF sprawdza, czy dane reprezentuje jednostkę już w śledzenie zmian dla wystąpienia kontekstu</span><span class="sxs-lookup"><span data-stu-id="0190a-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="0190a-115">Jeśli tak, zwracany jest istniejąca jednostka</span><span class="sxs-lookup"><span data-stu-id="0190a-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="0190a-116">W przeciwnym razie jest tworzona nowa jednostka skonfigurowano śledzenie zmian i zwracany jest nowa jednostka</span><span class="sxs-lookup"><span data-stu-id="0190a-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="0190a-117">Jeśli zapytania śledzenia nie EF sprawdza, czy dane reprezentuje jednostkę już w zestawu wyników dla tego zapytania</span><span class="sxs-lookup"><span data-stu-id="0190a-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="0190a-118">Jeśli tak, jest zwracana istniejącej jednostki <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="0190a-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="0190a-119">Jeśli nie, nowy obiekt jest tworzony i zwracany</span><span class="sxs-lookup"><span data-stu-id="0190a-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="0190a-120"><sup>(1) </sup> Nie śledzenia zapytań za pomocą słabe odwołania do śledzenie jednostek, które już zostały zwrócone.</span><span class="sxs-lookup"><span data-stu-id="0190a-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="0190a-121">Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres, a następnie uruchamia wyrzucanie elementów bezużytecznych, może pojawić się nowe wystąpienie jednostki.</span><span class="sxs-lookup"><span data-stu-id="0190a-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="0190a-122">Gdy zapytania są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="0190a-122">When queries are executed</span></span>

<span data-ttu-id="0190a-123">Po wywołaniu operatorów LINQ, są po prostu budowania reprezentacji w pamięci, zapytania.</span><span class="sxs-lookup"><span data-stu-id="0190a-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="0190a-124">Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy wyniki są używane.</span><span class="sxs-lookup"><span data-stu-id="0190a-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="0190a-125">Najbardziej typowe operacje, których wynikiem zapytania są wysyłane do bazy danych są:</span><span class="sxs-lookup"><span data-stu-id="0190a-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="0190a-126">Iteracja wyniki w parametrze `for` pętli</span><span class="sxs-lookup"><span data-stu-id="0190a-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="0190a-127">Przy użyciu operatora, takich jak `ToList`, `ToArray`, `Single`, `Count`</span><span class="sxs-lookup"><span data-stu-id="0190a-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="0190a-128">Powiązanie danych z wynikami zapytania do interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="0190a-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="0190a-129">**Zawsze weryfikowały dane wejściowe użytkownika:** EF podczas zapewniania ochrony przed atakami polegającymi na iniekcji SQL, nie ma żadnych ogólnej walidacji danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="0190a-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="0190a-130">W związku z tym jeśli wartości są przekazywane do interfejsów API, które są używane w kwerendach LINQ, przypisany do właściwości obiektu itd., pochodzi z niezaufanego źródła, a następnie odpowiednią sprawdzania poprawności, zgodnie z wymaganiami aplikacji powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="0190a-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="0190a-131">Dotyczy to danych podawanych przez użytkownika używane do dynamicznego utworzenia kwerendy.</span><span class="sxs-lookup"><span data-stu-id="0190a-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="0190a-132">Nawet wtedy, gdy za pomocą LINQ, jeśli akceptujesz, że dane wejściowe podane użytkownika, aby zbudować wyrażenia, potrzebne do upewnij się, niż tylko zamierzony wyrażenia można skonstruować.</span><span class="sxs-lookup"><span data-stu-id="0190a-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
