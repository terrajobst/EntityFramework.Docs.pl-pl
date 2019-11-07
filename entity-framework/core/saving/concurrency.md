---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: b72fa472698e76e18f155cf96b738b0e193eee0f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654615"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="4316b-102">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="4316b-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="4316b-103">Ta strona dokumentuje sposób działania współbieżności w EF Core oraz sposób obsługi konfliktów współbieżności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4316b-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="4316b-104">Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) , aby uzyskać szczegółowe informacje na temat konfigurowania tokenów współbieżności w modelu.</span><span class="sxs-lookup"><span data-stu-id="4316b-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="4316b-105">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="4316b-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="4316b-106">_Współbieżność bazy danych_ odnosi się do sytuacji, w których wiele procesów lub użytkowników uzyskuje dostęp lub zmienia te same dane w bazie danych w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="4316b-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="4316b-107">_Kontrola współbieżności_ odnosi się do określonych mechanizmów używanych do zapewnienia spójności danych w obecności współbieżnych zmian.</span><span class="sxs-lookup"><span data-stu-id="4316b-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="4316b-108">EF Core implementuje _optymistyczną kontrolę współbieżności_, co oznacza, że umożliwia wiele procesów lub użytkownikom wprowadzanie zmian niezależnie od obciążenia synchronizacji lub blokowania.</span><span class="sxs-lookup"><span data-stu-id="4316b-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="4316b-109">W przypadku idealnej sytuacji te zmiany nie będą zakłócać działania i w związku z tym będą mogły się powieść.</span><span class="sxs-lookup"><span data-stu-id="4316b-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="4316b-110">W scenariuszu najgorszego przypadku co najmniej dwa procesy będą próbować wprowadzać zmiany w konflikcie, a tylko jeden z nich powinien pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="4316b-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="4316b-111">Jak działa kontrola współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="4316b-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="4316b-112">Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania optymistycznej kontroli współbieżności: za każdym razem, gdy operacja aktualizowania lub usuwania jest wykonywana podczas `SaveChanges`, wartość tokenu współbieżności w bazie danych jest porównywana z odczytaniem oryginalnej wartości. przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="4316b-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="4316b-113">Jeśli wartości są takie same, operacja może zostać ukończona.</span><span class="sxs-lookup"><span data-stu-id="4316b-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="4316b-114">Jeśli wartości nie są zgodne, EF Core zakłada, że inny użytkownik wykonał operację powodującą konflikt i przerywa bieżącą transakcję.</span><span class="sxs-lookup"><span data-stu-id="4316b-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="4316b-115">Sytuacja, w której inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją, jest znana jako _konflikt współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="4316b-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="4316b-116">Dostawcy baz danych są odpowiedzialni za implementację porównania wartości tokenów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4316b-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="4316b-117">W relacyjnych bazach danych EF Core obejmuje sprawdzenie wartości tokenu współbieżności w klauzuli `WHERE` instrukcji `UPDATE` lub `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="4316b-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="4316b-118">Po wykonaniu instrukcji EF Core odczytuje liczbę wierszy, których to dotyczy.</span><span class="sxs-lookup"><span data-stu-id="4316b-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="4316b-119">Jeśli nie wpłynie to na żadne wiersze, zostanie wykryte konflikt współbieżności, a EF Core generuje `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="4316b-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="4316b-120">Na przykład możemy chcieć skonfigurować `LastName` na `Person` jako token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4316b-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="4316b-121">Następnie każda operacja aktualizacji dla osoby będzie obejmować kontrolę współbieżności w klauzuli `WHERE`:</span><span class="sxs-lookup"><span data-stu-id="4316b-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="4316b-122">Rozwiązywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="4316b-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="4316b-123">Kontynuując poprzedni przykład, jeśli jeden użytkownik próbuje zapisać pewne zmiany w `Person`, ale inny użytkownik zmienił już `LastName`, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="4316b-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="4316b-124">W tym momencie aplikacja może po prostu poinformować użytkownika o tym, że aktualizacja nie powiodła się z powodu sprzecznych zmian i przejścia.</span><span class="sxs-lookup"><span data-stu-id="4316b-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="4316b-125">Może jednak być wskazane monitowanie użytkownika o zapewnienie, że ten rekord nadal reprezentuje tę samą osobę rzeczywistą i spróbuje ponownie wykonać operację.</span><span class="sxs-lookup"><span data-stu-id="4316b-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="4316b-126">Ten proces jest przykładem _rozwiązania konfliktu współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="4316b-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="4316b-127">Rozwiązanie konfliktu współbieżności polega na scaleniu oczekujących zmian z bieżącego `DbContext` z wartościami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4316b-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="4316b-128">Jakie wartości są scalane, różnią się w zależności od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4316b-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="4316b-129">**Dostępne są trzy zestawy wartości, które ułatwiają rozwiązanie konfliktu współbieżności:**</span><span class="sxs-lookup"><span data-stu-id="4316b-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="4316b-130">**Bieżące wartości** to wartości, które aplikacja podjęła próbę zapisu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4316b-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="4316b-131">**Pierwotne wartości** to wartości, które zostały pierwotnie pobrane z bazy danych, przed wprowadzeniem jakichkolwiek zmian.</span><span class="sxs-lookup"><span data-stu-id="4316b-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="4316b-132">**Wartości bazy danych** są obecnie przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4316b-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="4316b-133">Ogólnym podejściem do obsłużenia konfliktów współbieżności jest:</span><span class="sxs-lookup"><span data-stu-id="4316b-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="4316b-134">`DbUpdateConcurrencyException` przechwytywania podczas `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4316b-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="4316b-135">Użyj `DbUpdateConcurrencyException.Entries`, aby przygotować nowy zestaw zmian dla odnośnych jednostek.</span><span class="sxs-lookup"><span data-stu-id="4316b-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="4316b-136">Odśwież oryginalne wartości tokenu współbieżności, aby odzwierciedlić bieżące wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4316b-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="4316b-137">Ponów próbę wykonania procesu, dopóki nie wystąpią żadne konflikty.</span><span class="sxs-lookup"><span data-stu-id="4316b-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="4316b-138">W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="4316b-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="4316b-139">Istnieje komentarz `// TODO:` w lokalizacji, w której dołączysz logikę specyficzną dla aplikacji w celu wybrania wartości do zapisania.</span><span class="sxs-lookup"><span data-stu-id="4316b-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
