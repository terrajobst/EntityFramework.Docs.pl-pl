---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: 4d6ff24e58caa0b228e9c1e4313beda78d1025fc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197832"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="4d055-102">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="4d055-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="4d055-103">Ta strona dokumentuje sposób działania współbieżności w EF Core oraz sposób obsługi konfliktów współbieżności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4d055-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="4d055-104">Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) , aby uzyskać szczegółowe informacje na temat konfigurowania tokenów współbieżności w modelu.</span><span class="sxs-lookup"><span data-stu-id="4d055-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="4d055-105">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="4d055-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="4d055-106">_Współbieżność bazy danych_ odnosi się do sytuacji, w których wiele procesów lub użytkowników uzyskuje dostęp lub zmienia te same dane w bazie danych w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="4d055-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="4d055-107">_Kontrola współbieżności_ odnosi się do określonych mechanizmów używanych do zapewnienia spójności danych w obecności współbieżnych zmian.</span><span class="sxs-lookup"><span data-stu-id="4d055-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="4d055-108">EF Core implementuje _optymistyczną kontrolę współbieżności_, co oznacza, że umożliwia wiele procesów lub użytkownikom wprowadzanie zmian niezależnie od obciążenia synchronizacji lub blokowania.</span><span class="sxs-lookup"><span data-stu-id="4d055-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="4d055-109">W przypadku idealnej sytuacji te zmiany nie będą zakłócać działania i w związku z tym będą mogły się powieść.</span><span class="sxs-lookup"><span data-stu-id="4d055-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="4d055-110">W scenariuszu najgorszego przypadku co najmniej dwa procesy będą próbować wprowadzać zmiany w konflikcie, a tylko jeden z nich powinien pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="4d055-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="4d055-111">Jak działa kontrola współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="4d055-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="4d055-112">Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania optymistycznej kontroli współbieżności: za każdym razem, gdy `SaveChanges`operacja aktualizowania lub usuwania jest wykonywana w trakcie, wartość tokenu współbieżności w bazie danych jest porównywana z oryginalną wartość odczytana przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="4d055-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="4d055-113">Jeśli wartości są takie same, operacja może zostać ukończona.</span><span class="sxs-lookup"><span data-stu-id="4d055-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="4d055-114">Jeśli wartości nie są zgodne, EF Core zakłada, że inny użytkownik wykonał operację powodującą konflikt i przerywa bieżącą transakcję.</span><span class="sxs-lookup"><span data-stu-id="4d055-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="4d055-115">Sytuacja, w której inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją, jest znana jako _konflikt współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="4d055-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="4d055-116">Dostawcy baz danych są odpowiedzialni za implementację porównania wartości tokenów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4d055-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="4d055-117">W relacyjnych bazach danych EF Core obejmuje sprawdzenie wartości tokenu współbieżności w `WHERE` klauzuli `UPDATE` instrukcji or `DELETE` .</span><span class="sxs-lookup"><span data-stu-id="4d055-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="4d055-118">Po wykonaniu instrukcji EF Core odczytuje liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="4d055-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="4d055-119">Jeśli nie wpłynie to na żadne wiersze, zostanie wykryte konflikt współbieżności, a `DbUpdateConcurrencyException`EF Core zgłasza.</span><span class="sxs-lookup"><span data-stu-id="4d055-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="4d055-120">Na przykład możemy chcieć skonfigurować `LastName` `Person` jako token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4d055-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="4d055-121">Następnie każda operacja aktualizacji dla osoby będzie obejmować kontrolę współbieżności w `WHERE` klauzuli:</span><span class="sxs-lookup"><span data-stu-id="4d055-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="4d055-122">Rozwiązywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="4d055-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="4d055-123">Kontynuując poprzedni przykład, jeśli jeden użytkownik próbuje zapisać pewne zmiany w `Person`, ale inny użytkownik już `LastName`zmienił, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4d055-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="4d055-124">W tym momencie aplikacja może po prostu poinformować użytkownika o tym, że aktualizacja nie powiodła się z powodu sprzecznych zmian i przejścia.</span><span class="sxs-lookup"><span data-stu-id="4d055-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="4d055-125">Może jednak być wskazane monitowanie użytkownika o zapewnienie, że ten rekord nadal reprezentuje tę samą osobę rzeczywistą i spróbuje ponownie wykonać operację.</span><span class="sxs-lookup"><span data-stu-id="4d055-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="4d055-126">Ten proces jest przykładem _rozwiązania konfliktu współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="4d055-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="4d055-127">Rozwiązanie konfliktu współbieżności polega na scaleniu oczekujących zmian z bieżącego `DbContext` z wartościami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4d055-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="4d055-128">Jakie wartości są scalane, różnią się w zależności od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4d055-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="4d055-129">**Dostępne są trzy zestawy wartości, które ułatwiają rozwiązanie konfliktu współbieżności:**</span><span class="sxs-lookup"><span data-stu-id="4d055-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="4d055-130">**Bieżące wartości** to wartości, które aplikacja podjęła próbę zapisu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4d055-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="4d055-131">**Pierwotne wartości** to wartości, które zostały pierwotnie pobrane z bazy danych, przed wprowadzeniem jakichkolwiek zmian.</span><span class="sxs-lookup"><span data-stu-id="4d055-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="4d055-132">**Wartości bazy danych** są obecnie przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4d055-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="4d055-133">Ogólnym podejściem do obsłużenia konfliktów współbieżności jest:</span><span class="sxs-lookup"><span data-stu-id="4d055-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="4d055-134">Catch `DbUpdateConcurrencyException` w `SaveChanges`trakcie.</span><span class="sxs-lookup"><span data-stu-id="4d055-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="4d055-135">Służy `DbUpdateConcurrencyException.Entries` do przygotowywania nowego zestawu zmian dla odnośnych jednostek.</span><span class="sxs-lookup"><span data-stu-id="4d055-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="4d055-136">Odśwież oryginalne wartości tokenu współbieżności, aby odzwierciedlić bieżące wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4d055-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="4d055-137">Ponów próbę wykonania procesu, dopóki nie wystąpią żadne konflikty.</span><span class="sxs-lookup"><span data-stu-id="4d055-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="4d055-138">W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4d055-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="4d055-139">`// TODO:` Istnieje komentarz w lokalizacji, w której dołączysz logikę specyficzną dla aplikacji w celu wybrania wartości do zapisania.</span><span class="sxs-lookup"><span data-stu-id="4d055-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
