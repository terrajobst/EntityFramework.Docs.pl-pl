---
title: Obsługa konfliktów współbieżności — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417591"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="d04e1-102">Obsługa konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="d04e1-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="d04e1-103">Ta strona dokumentuje, jak współbieżność działa w EF Core i jak obsługiwać konflikty współbieżności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d04e1-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="d04e1-104">Zobacz [tokeny współbieżności,](xref:core/modeling/concurrency) aby uzyskać szczegółowe informacje na temat konfigurowania tokenów współbieżności w modelu.</span><span class="sxs-lookup"><span data-stu-id="d04e1-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="d04e1-105">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="d04e1-105">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="d04e1-106">_Współbieżność bazy danych_ odnosi się do sytuacji, w których wiele procesów lub użytkowników dostęp lub zmienić te same dane w bazie danych w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="d04e1-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="d04e1-107">_Kontrola współbieżności_ odnosi się do określonych mechanizmów używanych w celu zapewnienia spójności danych w obecności równoczesnych zmian.</span><span class="sxs-lookup"><span data-stu-id="d04e1-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="d04e1-108">EF Core implementuje _kontrolę współbieżności optymistyczne,_ co oznacza, że pozwoli wielu procesów lub użytkowników wprowadzać zmiany niezależnie bez narzutu synchronizacji lub blokowania.</span><span class="sxs-lookup"><span data-stu-id="d04e1-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="d04e1-109">W idealnej sytuacji zmiany te nie będą kolidować ze sobą i dlatego będą w stanie odnieść sukces.</span><span class="sxs-lookup"><span data-stu-id="d04e1-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="d04e1-110">W najgorszym przypadku dwa lub więcej procesów będzie próbował wprowadzić zmiany powodujące konflikty i tylko jeden z nich powinien zakończyć się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="d04e1-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="d04e1-111">Jak działa kontrola współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="d04e1-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="d04e1-112">Właściwości skonfigurowane jako tokeny współbieżności są używane do implementowania kontroli współbieżności optymistyczne: `SaveChanges`za każdym razem, gdy operacja aktualizacji lub usuwania jest wykonywana podczas , wartość tokenu współbieżności w bazie danych jest porównywana z oryginalną wartością odczytywaną przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="d04e1-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="d04e1-113">Jeśli wartości są zgodne, operacja może zakończyć.</span><span class="sxs-lookup"><span data-stu-id="d04e1-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="d04e1-114">Jeśli wartości nie są zgodne, EF Core zakłada, że inny użytkownik wykonał operację powodującą konflikt i przerywa bieżącą transakcję.</span><span class="sxs-lookup"><span data-stu-id="d04e1-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="d04e1-115">Sytuacja, gdy inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją jest znany jako _konflikt współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="d04e1-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="d04e1-116">Dostawcy bazy danych są odpowiedzialni za implementację porównania wartości tokenów współbieżności.</span><span class="sxs-lookup"><span data-stu-id="d04e1-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="d04e1-117">W relacyjnych bazach danych EF Core zawiera sprawdzenie wartości tokenu współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` instrukcji.</span><span class="sxs-lookup"><span data-stu-id="d04e1-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="d04e1-118">Po wykonaniu instrukcji EF Core odczytuje liczbę wierszy, których dotyczy problem.</span><span class="sxs-lookup"><span data-stu-id="d04e1-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="d04e1-119">Jeśli nie dotyczy to żadnych wierszy, zostanie wykryty konflikt współbieżności, a ef core zostanie rzucony. `DbUpdateConcurrencyException`</span><span class="sxs-lookup"><span data-stu-id="d04e1-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="d04e1-120">Na przykład możemy chcieć `LastName` skonfigurować jako `Person` token współbieżności.</span><span class="sxs-lookup"><span data-stu-id="d04e1-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="d04e1-121">Następnie każda operacja aktualizacji na osobę będzie zawierać `WHERE` sprawdzenie współbieżności w klauzuli:</span><span class="sxs-lookup"><span data-stu-id="d04e1-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="d04e1-122">Rozwiązywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="d04e1-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="d04e1-123">Kontynuując poprzedni przykład, jeśli jeden użytkownik próbuje zapisać `Person`niektóre zmiany w , `LastName`ale inny użytkownik już zmienił , a następnie zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d04e1-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="d04e1-124">W tym momencie aplikacja może po prostu poinformować użytkownika, że aktualizacja nie powiodła się z powodu sprzecznych zmian i przejść dalej.</span><span class="sxs-lookup"><span data-stu-id="d04e1-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="d04e1-125">Ale może być pożądane, aby poprosić użytkownika, aby upewnić się, że ten rekord nadal reprezentuje tę samą rzeczywistą osobę i ponowić próbę wykonania operacji.</span><span class="sxs-lookup"><span data-stu-id="d04e1-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="d04e1-126">Ten proces jest przykładem _rozwiązania konfliktu współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="d04e1-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="d04e1-127">Rozwiązywanie konfliktu współbieżności polega na scalaniu oczekujących `DbContext` zmian z bieżącego z wartościami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d04e1-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="d04e1-128">Jakie wartości zostaną scalone różnią się w zależności od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d04e1-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="d04e1-129">**Dostępne są trzy zestawy wartości ułatwiające rozwiązanie konfliktu współbieżności:**</span><span class="sxs-lookup"><span data-stu-id="d04e1-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="d04e1-130">**Bieżące wartości** są wartościami, które aplikacja próbowała zapisać w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d04e1-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="d04e1-131">**Wartości oryginalne** są wartościami, które zostały pierwotnie pobrane z bazy danych, przed dokonaniem jakichkolwiek zmian.</span><span class="sxs-lookup"><span data-stu-id="d04e1-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="d04e1-132">**Wartości bazy danych** są wartościami aktualnie przechowywanymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d04e1-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="d04e1-133">Ogólne podejście do obsługi konfliktów współbieżności jest:</span><span class="sxs-lookup"><span data-stu-id="d04e1-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="d04e1-134">Złapać `DbUpdateConcurrencyException` `SaveChanges`podczas .</span><span class="sxs-lookup"><span data-stu-id="d04e1-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="d04e1-135">Służy `DbUpdateConcurrencyException.Entries` do przygotowania nowego zestawu zmian dla jednostek, których dotyczy problem.</span><span class="sxs-lookup"><span data-stu-id="d04e1-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="d04e1-136">Odśwież oryginalne wartości tokenu współbieżności, aby odzwierciedlić bieżące wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d04e1-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="d04e1-137">Ponów próbę wykonania procesu, dopóki nie wystąpią żadne konflikty.</span><span class="sxs-lookup"><span data-stu-id="d04e1-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="d04e1-138">W poniższym `Person.FirstName` przykładzie i `Person.LastName` są skonfigurowane jako tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="d04e1-138">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="d04e1-139">Istnieje `// TODO:` komentarz w lokalizacji, w której można dołączyć logikę specyficzne dla aplikacji, aby wybrać wartość do zapisania.</span><span class="sxs-lookup"><span data-stu-id="d04e1-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
