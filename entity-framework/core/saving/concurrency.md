---
title: "Obsługa konfliktom współbieżności - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="6ea11-102">Obsługa konfliktom współbieżności</span><span class="sxs-lookup"><span data-stu-id="6ea11-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="6ea11-103">Ta strona dokumentów działania współbieżności EF Core i sposób obsługi konfliktom współbieżności w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ea11-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="6ea11-104">Zobacz [tokeny współbieżności](xref:core/modeling/concurrency) szczegółowe informacje na temat konfigurowania tokeny współbieżności w modelu.</span><span class="sxs-lookup"><span data-stu-id="6ea11-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="6ea11-105">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="6ea11-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="6ea11-106">_Baza danych współbieżności_ odwołuje się do sytuacji, w których wiele procesów lub użytkowników dostęp lub zmienić tych samych danych w bazie danych, w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="6ea11-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="6ea11-107">_Formant współbieżności_ odwołuje się do określonych mechanizmy służące do zapewnienia spójności danych w obecności równoczesnych zmian.</span><span class="sxs-lookup"><span data-stu-id="6ea11-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="6ea11-108">Implementuje EF Core _optymistyczne sterowanie współbieżnością_, co oznacza, że umożliwi programowi wiele procesów lub użytkownicy wprowadzać zmiany niezależnie bez ponoszenia dodatkowych nakładów synchronizacji lub blokowania.</span><span class="sxs-lookup"><span data-stu-id="6ea11-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="6ea11-109">W sytuacji idealnej tych zmian nie kolidują ze sobą i w związku z tym będzie można pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="6ea11-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="6ea11-110">W scenariusz dwie lub więcej procesów podejmie próbę zmiany powodujące konflikt, i tylko jeden z nich ma być pomyślnie wykonane.</span><span class="sxs-lookup"><span data-stu-id="6ea11-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="6ea11-111">Jak działa formant współbieżności w EF Core</span><span class="sxs-lookup"><span data-stu-id="6ea11-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="6ea11-112">Właściwości skonfigurowany jako tokeny współbieżności są używane do implementowania optymistyczne sterowanie współbieżnością: zawsze, gdy jest wykonywana operacja aktualizacji lub usuwania podczas `SaveChanges`, wartość tokenu współbieżności bazy danych zostaną porównane z oryginalną wartość odczytywane przez EF Core.</span><span class="sxs-lookup"><span data-stu-id="6ea11-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="6ea11-113">Jeśli wartości są zgodne, można wykonać operacji.</span><span class="sxs-lookup"><span data-stu-id="6ea11-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="6ea11-114">Jeśli wartości nie są zgodne, EF Core założono, że inny użytkownik wykonał operacji powodującej konflikt i przerwanie bieżącej transakcji.</span><span class="sxs-lookup"><span data-stu-id="6ea11-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="6ea11-115">Sytuacji, gdy inny użytkownik wykonał operację, która powoduje konflikt z bieżącą operacją nosi nazwę _konflikt współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="6ea11-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="6ea11-116">Dostawcy bazy danych są zobowiązani do wykonania porównania wartości tokenu współbieżności.</span><span class="sxs-lookup"><span data-stu-id="6ea11-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="6ea11-117">W relacyjnych baz danych EF Core obejmuje sprawdzenia wartości tokenu współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` instrukcje.</span><span class="sxs-lookup"><span data-stu-id="6ea11-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="6ea11-118">Po wykonaniu instrukcji, EF Core odczytuje liczbę wierszy, które zostały zainfekowane.</span><span class="sxs-lookup"><span data-stu-id="6ea11-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="6ea11-119">Jeśli dotyczy żadnych wierszy, został wykryty konflikt współbieżności i zgłasza EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6ea11-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="6ea11-120">Na przykład firma Microsoft może być konieczne skonfigurowanie `LastName` na `Person` być tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="6ea11-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="6ea11-121">Następnie żadnej operacji update na osoba będzie zawierać wyboru współbieżność w `WHERE` klauzuli:</span><span class="sxs-lookup"><span data-stu-id="6ea11-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="6ea11-122">Rozwiązywanie konfliktów współbieżności</span><span class="sxs-lookup"><span data-stu-id="6ea11-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="6ea11-123">Kontynuowanie poprzedni przykład, jeśli jeden użytkownik próbuje zapisać pewnych zmian `Person`, ale już inny użytkownik zmienił `LastName` zostanie wygenerowany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="6ea11-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="6ea11-124">W tym momencie aplikacja może po prostu poinformować użytkownika, że aktualizacja nie zakończyła się niepowodzeniem z powodu zmian powodujących konflikt i przenieść na.</span><span class="sxs-lookup"><span data-stu-id="6ea11-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="6ea11-125">Jednak może być pożądane, aby monitować użytkownika, upewnij się, że ten rekord nadal reprezentuje sama osoba rzeczywiste i spróbuj ponownie wykonać operację.</span><span class="sxs-lookup"><span data-stu-id="6ea11-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="6ea11-126">Ten proces jest przykładem _Rozwiązywanie konfliktu współbieżności_.</span><span class="sxs-lookup"><span data-stu-id="6ea11-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="6ea11-127">Rozwiązywanie konfliktów współbieżności obejmuje scalanie oczekujących zmian z bieżącego `DbContext` z wartościami w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6ea11-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="6ea11-128">Jakie wartości scalony różni się zależnie od aplikacji i mogą być kierowane przez dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6ea11-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="6ea11-129">**Istnieją trzy zestawy wartości może pomóc rozwiązać konflikt współbieżności:**</span><span class="sxs-lookup"><span data-stu-id="6ea11-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="6ea11-130">**Bieżące wartości** wartości próby zapisu w bazie danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ea11-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="6ea11-131">**Oryginalne wartości** to wartości, które zostały pierwotnie pobrany z bazy danych, aby zmiany zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="6ea11-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="6ea11-132">**Baza danych wartości** wartości przechowywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6ea11-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="6ea11-133">Ogólne podejście do obsługi konfliktom współbieżności jest:</span><span class="sxs-lookup"><span data-stu-id="6ea11-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="6ea11-134">CATCH `DbUpdateConcurrencyException` podczas `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="6ea11-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="6ea11-135">Użyj `DbUpdateConcurrencyException.Entries` Aby przygotować nowy zestaw zmian do odpowiednich jednostek.</span><span class="sxs-lookup"><span data-stu-id="6ea11-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="6ea11-136">Odśwież oryginalnych wartości tokenu współbieżności do bieżącej wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6ea11-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="6ea11-137">Ponów próbę wykonania procesu, dopóki nie występują żadne konflikty.</span><span class="sxs-lookup"><span data-stu-id="6ea11-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="6ea11-138">W poniższym przykładzie `Person.FirstName` i `Person.LastName` są skonfigurowane jako tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="6ea11-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="6ea11-139">Brak `// TODO:` — komentarz w lokalizacji, gdzie obejmują określonego logikę aplikacji, aby wybrać wartość do zapisania.</span><span class="sxs-lookup"><span data-stu-id="6ea11-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
