---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: e050b17bfa31a4785161c700bc0355e83162b405
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993115"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="81f20-102">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="81f20-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="81f20-103">Ta strona dokumentów, jak współbieżność działa w programie EF Core i sposób obsługi konfliktów współbieżności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81f20-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="81f20-104">Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) Aby uzyskać szczegółowe informacje na temat konfigurowania tokeny współbieżności w modelu.</span><span class="sxs-lookup"><span data-stu-id="81f20-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="81f20-105">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="81f20-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="81f20-106">_Baza danych współbieżności_ odnosi się do sytuacji, w których wiele procesów lub użytkownikom dostęp lub zmienić te same dane w bazie danych, w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="81f20-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="81f20-107">_Kontrola współbieżności_ odwołuje się do określonych mechanizmów używane w celu zapewnienia spójności danych w obecności równoczesnych zmian.</span><span class="sxs-lookup"><span data-stu-id="81f20-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="81f20-108">Implementuje programu EF Core _mechanizmu kontroli optymistycznej współbieżności_, co oznacza, że umożliwi ona wiele procesów lub użytkownicy wprowadzać zmiany, niezależnie od siebie bez konieczności synchronizacji lub blokowania.</span><span class="sxs-lookup"><span data-stu-id="81f20-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="81f20-109">W sytuacji idealnej te zmiany nie kolidują ze sobą i w związku z tym będzie można pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="81f20-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="81f20-110">W najgorszym przypadku wielkości liter dwa lub więcej procesów podejmie próbę zmiany powodujące konflikt, i tylko jeden z nich ma być pomyślnie wykonane.</span><span class="sxs-lookup"><span data-stu-id="81f20-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="81f20-111">Jak działa Kontrola współbieżności w programie EF Core</span><span class="sxs-lookup"><span data-stu-id="81f20-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="81f20-112">Właściwości skonfigurowane tokeny współbieżności są używane do wdrożenia mechanizmu kontroli optymistycznej współbieżności: zawsze, gdy operacji update lub delete jest wykonywane podczas `SaveChanges`, wartość tokenu współbieżności w bazie danych jest porównywana oryginał wartość odczytu przez platformę EF Core.</span><span class="sxs-lookup"><span data-stu-id="81f20-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="81f20-113">Jeśli wartości są zgodne, można wykonać operacji.</span><span class="sxs-lookup"><span data-stu-id="81f20-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="81f20-114">Jeśli wartości nie są zgodne, EF Core założono, że inny użytkownik wykonał operacji powodującej konflikt i przerywa bieżącej transakcji.</span><span class="sxs-lookup"><span data-stu-id="81f20-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="81f20-115">Sytuacja, gdy inny użytkownik wykonał operację, która powoduje konflikt z bieżącej operacji jest znany jako _konfliktów współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="81f20-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="81f20-116">Dostawcy baz danych są odpowiedzialni za wdrażanie porównanie wartości tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="81f20-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="81f20-117">W relacyjnych baz danych programu EF Core obejmuje sprawdzenia wartości tokenu współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="81f20-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="81f20-118">Po wykonaniu instrukcji, programem EF Core odczytuje liczbę wierszy, które miały wpływ.</span><span class="sxs-lookup"><span data-stu-id="81f20-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="81f20-119">Jeśli wpływają żadne wiersze nie został wykryty konflikt współbieżności i zgłasza wyjątek programu EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="81f20-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="81f20-120">Na przykład firma Microsoft może być konieczne skonfigurowanie `LastName` na `Person` za tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="81f20-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="81f20-121">Następnie żadnych operacji aktualizacji w osoba będzie zawierać wyboru współbieżność w `WHERE` klauzuli:</span><span class="sxs-lookup"><span data-stu-id="81f20-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="81f20-122">Rozwiązywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="81f20-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="81f20-123">Kontynuując poprzedni przykład gdy jeden użytkownik próbuje zapisać pewne zmiany `Person`, ale już inny użytkownik zmienił `LastName`, a następnie zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="81f20-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="81f20-124">W tym momencie aplikacja może po prostu informować użytkownika, że aktualizacja zakończyła się niepowodzeniem z powodu zmian powodujących konflikt i przejść.</span><span class="sxs-lookup"><span data-stu-id="81f20-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="81f20-125">Jednak może być pożądane, aby monitować użytkownika, upewnij się, że ten rekord reprezentuje nadal tę samą osobę rzeczywiste i spróbuj ponownie wykonać operację.</span><span class="sxs-lookup"><span data-stu-id="81f20-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="81f20-126">Ten proces jest przykładem _Rozwiązywanie konfliktów współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="81f20-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="81f20-127">Rozwiązywanie konfliktów współbieżności obejmuje scalanie oczekujące zmiany z bieżącej `DbContext` z wartościami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="81f20-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="81f20-128">Scalony jakie wartości będą się różnić w zależności od aplikacji i zaleceniami dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="81f20-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="81f20-129">**Istnieją trzy rodzaje wartości może pomóc rozwiązać konflikt współbieżności:**</span><span class="sxs-lookup"><span data-stu-id="81f20-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="81f20-130">**Bieżące wartości** są wartościami, które aplikacja próby zapisu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="81f20-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="81f20-131">**Oryginalne wartości** są wartościami, które zostały pierwotnie pobrany z bazy danych, zanim zmiany zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="81f20-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="81f20-132">**Bazy danych wartości** wartości przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="81f20-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="81f20-133">Obsługa konfliktów współbieżności ogólne podejście jest:</span><span class="sxs-lookup"><span data-stu-id="81f20-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="81f20-134">CATCH `DbUpdateConcurrencyException` podczas `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="81f20-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="81f20-135">Użyj `DbUpdateConcurrencyException.Entries` Aby przygotować nowy zestaw zmian dla obiektów, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="81f20-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="81f20-136">Odśwież oryginalne wartości parametru tokenu współbieżności do bieżącej wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="81f20-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="81f20-137">Ponów próbę wykonania procesu, dopóki nie występują żadne konflikty.</span><span class="sxs-lookup"><span data-stu-id="81f20-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="81f20-138">W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="81f20-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="81f20-139">Brak `// TODO:` komentarz w lokalizacji, gdzie obejmują określonej logiki aplikacji, aby wybrać wartość do zapisania.</span><span class="sxs-lookup"><span data-stu-id="81f20-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
