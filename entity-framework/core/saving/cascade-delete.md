---
title: Kaskadowe usuwanie EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417615"
---
# <a name="cascade-delete"></a><span data-ttu-id="229b4-102">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="229b4-102">Cascade Delete</span></span>

<span data-ttu-id="229b4-103">Funkcja usuwania kaskadowego jest często używana w terminologii bazy danych do opisywania cechy, która umożliwia usunięcie wiersza w celu automatycznego wyzwalania usuwania powiązanych wierszy.</span><span class="sxs-lookup"><span data-stu-id="229b4-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="229b4-104">Silnie powiązane koncepcje objęte również EF Core zachowaniem usunięcia to automatyczne usunięcie jednostki podrzędnej, gdy jest ona relacją z elementem nadrzędnym — jest to często nazywane "usuwaniem sierotów".</span><span class="sxs-lookup"><span data-stu-id="229b4-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="229b4-105">EF Core implementuje kilka różnych zachowań usuwania i umożliwia konfigurację zachowań poszczególnych relacji.</span><span class="sxs-lookup"><span data-stu-id="229b4-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="229b4-106">EF Core również implementuje konwencje, które automatycznie skonfigurują domyślne zachowania usuwania dla każdej relacji w zależności od [wymaganej relacji](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="229b4-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="229b4-107">Usuwanie zachowań</span><span class="sxs-lookup"><span data-stu-id="229b4-107">Delete behaviors</span></span>

<span data-ttu-id="229b4-108">Zachowania usuwania są zdefiniowane w typie modułu wyliczającego *DeleteBehavior* i mogą być przenoszone do interfejsu API Fluent przy użyciu metody *onDelete* , aby określić, czy usunięcie podmiotu zabezpieczeń/jednostki nadrzędnej lub nawiązanie relacji z jednostkami zależnymi/podrzędnymi powinno mieć efekt uboczny dla jednostek zależnych/podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="229b4-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="229b4-109">Istnieją trzy akcje, które można wykonać, gdy jednostka główna/nadrzędna jest usuwana lub relacja do elementu podrzędnego jest poważna:</span><span class="sxs-lookup"><span data-stu-id="229b4-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="229b4-110">Można usunąć element podrzędny/zależny</span><span class="sxs-lookup"><span data-stu-id="229b4-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="229b4-111">Wartości klucza obcego dziecka można ustawić na wartość null.</span><span class="sxs-lookup"><span data-stu-id="229b4-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="229b4-112">Element podrzędny pozostaje niezmieniony</span><span class="sxs-lookup"><span data-stu-id="229b4-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="229b4-113">Zachowanie usuwania skonfigurowane w modelu EF Core jest stosowane tylko wtedy, gdy jednostka główna jest usuwana przy użyciu EF Core i jednostki zależne są ładowane w pamięci (czyli dla śledzonych elementów zależnych).</span><span class="sxs-lookup"><span data-stu-id="229b4-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="229b4-114">Odpowiednie zachowanie kaskadowe musi być konfiguracją w bazie danych, aby upewnić się, że dane, które nie są śledzone przez kontekst, mają zastosowane wymagane działanie.</span><span class="sxs-lookup"><span data-stu-id="229b4-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="229b4-115">Jeśli używasz EF Core do tworzenia bazy danych, to zachowanie kaskadowe zostanie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="229b4-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="229b4-116">Dla drugiej czynności powyżej ustawienie wartości klucza obcego na null jest nieprawidłowe, jeśli klucz obcy nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="229b4-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="229b4-117">(Klucz obcy niedopuszczający wartości null jest równoważny z wymaganą relacją). W takich przypadkach EF Core śledzi, że właściwość klucza obcego została oznaczona jako null do momentu wywołania metody SaveChanges, podczas gdy wyjątek jest zgłaszany, ponieważ zmiana nie może zostać utrwalona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="229b4-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="229b4-118">Jest to podobne do uzyskiwania naruszenia ograniczenia z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="229b4-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="229b4-119">Istnieją cztery zachowania dotyczące usuwania, jak pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="229b4-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="229b4-120">Opcjonalne relacje</span><span class="sxs-lookup"><span data-stu-id="229b4-120">Optional relationships</span></span>

<span data-ttu-id="229b4-121">W przypadku opcjonalnej relacji (klucz obcy dopuszczający wartość null _) można zapisać_ wartość null klucza obcego, co spowoduje następujące skutki:</span><span class="sxs-lookup"><span data-stu-id="229b4-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="229b4-122">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="229b4-122">Behavior Name</span></span>               | <span data-ttu-id="229b4-123">Efekt zależny/podrzędny w pamięci</span><span class="sxs-lookup"><span data-stu-id="229b4-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="229b4-124">Efekt zależny/podrzędny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="229b4-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="229b4-125">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="229b4-125">**Cascade**</span></span>                 | <span data-ttu-id="229b4-126">Jednostki zostały usunięte</span><span class="sxs-lookup"><span data-stu-id="229b4-126">Entities are deleted</span></span>                   | <span data-ttu-id="229b4-127">Jednostki zostały usunięte</span><span class="sxs-lookup"><span data-stu-id="229b4-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="229b4-128">**ClientSetNull** (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="229b4-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="229b4-129">Właściwości klucza obcego są ustawione na wartość null.</span><span class="sxs-lookup"><span data-stu-id="229b4-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="229b4-130">None</span><span class="sxs-lookup"><span data-stu-id="229b4-130">None</span></span>                                   |
| <span data-ttu-id="229b4-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="229b4-131">**SetNull**</span></span>                 | <span data-ttu-id="229b4-132">Właściwości klucza obcego są ustawione na wartość null.</span><span class="sxs-lookup"><span data-stu-id="229b4-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="229b4-133">Właściwości klucza obcego są ustawione na wartość null.</span><span class="sxs-lookup"><span data-stu-id="229b4-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="229b4-134">**Ograniczone**</span><span class="sxs-lookup"><span data-stu-id="229b4-134">**Restrict**</span></span>                | <span data-ttu-id="229b4-135">None</span><span class="sxs-lookup"><span data-stu-id="229b4-135">None</span></span>                                   | <span data-ttu-id="229b4-136">None</span><span class="sxs-lookup"><span data-stu-id="229b4-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="229b4-137">Wymagane relacje</span><span class="sxs-lookup"><span data-stu-id="229b4-137">Required relationships</span></span>

<span data-ttu-id="229b4-138">W przypadku wymaganych relacji (klucz obcy niedopuszczający wartości null) _nie_ można zapisać wartości null klucza obcego, co spowoduje następujące skutki:</span><span class="sxs-lookup"><span data-stu-id="229b4-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="229b4-139">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="229b4-139">Behavior Name</span></span>         | <span data-ttu-id="229b4-140">Efekt zależny/podrzędny w pamięci</span><span class="sxs-lookup"><span data-stu-id="229b4-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="229b4-141">Efekt zależny/podrzędny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="229b4-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="229b4-142">**Kaskada** (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="229b4-142">**Cascade** (Default)</span></span> | <span data-ttu-id="229b4-143">Jednostki zostały usunięte</span><span class="sxs-lookup"><span data-stu-id="229b4-143">Entities are deleted</span></span>                | <span data-ttu-id="229b4-144">Jednostki zostały usunięte</span><span class="sxs-lookup"><span data-stu-id="229b4-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="229b4-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="229b4-145">**ClientSetNull**</span></span>     | <span data-ttu-id="229b4-146">Metody SaveChanges zgłasza</span><span class="sxs-lookup"><span data-stu-id="229b4-146">SaveChanges throws</span></span>                  | <span data-ttu-id="229b4-147">None</span><span class="sxs-lookup"><span data-stu-id="229b4-147">None</span></span>                                  |
| <span data-ttu-id="229b4-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="229b4-148">**SetNull**</span></span>           | <span data-ttu-id="229b4-149">Metody SaveChanges zgłasza</span><span class="sxs-lookup"><span data-stu-id="229b4-149">SaveChanges throws</span></span>                  | <span data-ttu-id="229b4-150">Metody SaveChanges zgłasza</span><span class="sxs-lookup"><span data-stu-id="229b4-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="229b4-151">**Ograniczone**</span><span class="sxs-lookup"><span data-stu-id="229b4-151">**Restrict**</span></span>          | <span data-ttu-id="229b4-152">None</span><span class="sxs-lookup"><span data-stu-id="229b4-152">None</span></span>                                | <span data-ttu-id="229b4-153">None</span><span class="sxs-lookup"><span data-stu-id="229b4-153">None</span></span>                                  |

<span data-ttu-id="229b4-154">W podanych powyżej tabelach *żaden* z nich może spowodować naruszenie ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="229b4-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="229b4-155">Na przykład, jeśli jednostka główna/podrzędna jest usuwana, ale nie jest podejmowana żadna akcja w celu zmiany klucza obcego elementu zależnego/podrzędnego, baza danych prawdopodobnie zgłosi się na metody SaveChanges z powodu naruszenia ograniczenia obcego.</span><span class="sxs-lookup"><span data-stu-id="229b4-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="229b4-156">Na wysokim poziomie:</span><span class="sxs-lookup"><span data-stu-id="229b4-156">At a high level:</span></span>

* <span data-ttu-id="229b4-157">Jeśli masz jednostki, które nie mogą istnieć bez elementu nadrzędnego, i chcesz, aby program Dr zadbać o automatyczne usunięcie elementów podrzędnych, a następnie użyj opcji *kaskadowych*.</span><span class="sxs-lookup"><span data-stu-id="229b4-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="229b4-158">Jednostki, które nie mogą istnieć bez elementu nadrzędnego zwykle korzystają z wymaganych relacji, dla których *Kaskada* jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="229b4-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="229b4-159">Jeśli masz jednostki, które mogą lub nie mają elementu nadrzędnego, i chcesz, aby EF zadbać o wyzerowanie klucza obcego, a następnie użyj *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="229b4-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="229b4-160">Jednostki, które mogą istnieć bez elementu nadrzędnego zwykle używają relacji opcjonalnych, dla których *ClientSetNull* jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="229b4-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="229b4-161">Jeśli chcesz, aby baza danych mogła próbować propagować wartości null do podrzędnych kluczy obcych, nawet gdy jednostka podrzędna nie została załadowana, użyj właściwości *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="229b4-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="229b4-162">Należy jednak pamiętać, że ta baza danych programu musi obsługiwać ten program, a Konfiguracja bazy danych może spowodować inne ograniczenia, które w praktyce często powodują, że ta opcja jest nieprzydatna.</span><span class="sxs-lookup"><span data-stu-id="229b4-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="229b4-163">To dlatego, że *SetNull* nie jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="229b4-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="229b4-164">Jeśli nie chcesz, aby EF Core kiedykolwiek usunąć jednostki automatycznie lub wyzerować klucz obcy automatycznie, użyj opcji *Ogranicz*.</span><span class="sxs-lookup"><span data-stu-id="229b4-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="229b4-165">Należy zauważyć, że wymaga to, aby kod utrzymywać jednostki podrzędne i ich wartości klucza obcego w synchronizacji ręcznie w przeciwnym razie zostaną zgłoszone wyjątki ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="229b4-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="229b4-166">W EF Core, w przeciwieństwie do EF6, efekty kaskadowe nie są wykonywane natychmiast, ale tylko wtedy, gdy jest wywoływana metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="229b4-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="229b4-167">**Zmiany w EF Core 2,0:** W poprzednich wersjach *ograniczenie* mogłoby spowodować, że właściwości opcjonalnego klucza obcego w śledzonych jednostkach zależnych mają być ustawione na wartość null, i było domyślnym zachowaniem usuwania dla relacji opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="229b4-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="229b4-168">W EF Core 2,0 został wprowadzony *ClientSetNull* do reprezentowania tego zachowania i stał się wartością domyślną dla relacji opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="229b4-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="229b4-169">Zachowanie *ograniczenia* zostało dostosowane do wartości nigdy nie mają żadnych efektów ubocznych w jednostkach zależnych.</span><span class="sxs-lookup"><span data-stu-id="229b4-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="229b4-170">Przykłady usuwania jednostek</span><span class="sxs-lookup"><span data-stu-id="229b4-170">Entity deletion examples</span></span>

<span data-ttu-id="229b4-171">Poniższy kod jest częścią [próbki](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , którą można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="229b4-171">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="229b4-172">Przykład pokazuje, co się stanie w przypadku każdego zachowania usuwania zarówno dla relacji opcjonalnych, jak i wymaganych, gdy jednostka nadrzędna jest usuwana.</span><span class="sxs-lookup"><span data-stu-id="229b4-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="229b4-173">Zapoznaj się z informacjami o tym, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="229b4-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="229b4-174">DeleteBehavior. Kaskada z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="229b4-175">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="229b4-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="229b4-176">Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="229b4-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="229b4-177">Metody SaveChanges wysyła usunięcia zarówno dla elementów zależnych, jak i podrzędnych (wpisów), a następnie podmiotu zabezpieczeń/elementu nadrzędnego (blog)</span><span class="sxs-lookup"><span data-stu-id="229b4-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="229b4-178">Po zapisaniu wszystkie jednostki są odłączone, ponieważ zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="229b4-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="229b4-179">DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z wymaganą relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="229b4-180">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="229b4-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="229b4-181">Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="229b4-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="229b4-182">Metody SaveChanges próbuje ustawić wartość null dla wpisu klucza obcego, ale to nie powiodło się, ponieważ obcy nie dopuszcza wartości null</span><span class="sxs-lookup"><span data-stu-id="229b4-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="229b4-183">DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="229b4-184">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="229b4-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="229b4-185">Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="229b4-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="229b4-186">Metody SaveChanges próbuje przed usunięciem podmiotu zabezpieczeń/elementu nadrzędnego (ogłoszenia) do wartości null</span><span class="sxs-lookup"><span data-stu-id="229b4-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="229b4-187">Po zapisaniu podmiot zabezpieczeń/nadrzędne (blog) zostanie usunięty, ale zależności/elementy podrzędne (wpisy) są nadal śledzone</span><span class="sxs-lookup"><span data-stu-id="229b4-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="229b4-188">Śledzone zależności/elementy podrzędne (wpisy) mają teraz wartości klucza obcego i ich odwołanie do usuniętego podmiotu zabezpieczeń/nadrzędnego (blog) zostało usunięte</span><span class="sxs-lookup"><span data-stu-id="229b4-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="229b4-189">DeleteBehavior. Ogranicz z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="229b4-190">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="229b4-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="229b4-191">Wpisy pozostają początkowo niezmienione, ponieważ kaskady nie są wykonywane do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="229b4-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="229b4-192">Ponieważ wartość *ograniczenia* powoduje, że EF nie ustawia automatycznie klucza obcego na null, pozostaje nierówna null i metody SaveChanges zgłasza bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="229b4-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="229b4-193">Usuń przykłady oddzielonych</span><span class="sxs-lookup"><span data-stu-id="229b4-193">Delete orphans examples</span></span>

<span data-ttu-id="229b4-194">Poniższy kod jest częścią [próbki](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) , którą można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="229b4-194">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="229b4-195">Przykład pokazuje, co się stanie w przypadku każdego zachowania usuwania zarówno dla relacji opcjonalnych, jak i wymaganych, gdy relacja między obiektem nadrzędnym/podmiotem zabezpieczeń a jego elementami podrzędnymi/zależnymi jest poważna.</span><span class="sxs-lookup"><span data-stu-id="229b4-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="229b4-196">W tym przykładzie relacja jest porzucana przez usunięcie elementów zależnych/podrzędnych (wpisów) z właściwości nawigacji kolekcji na serwerze głównym/nadrzędnym (blog).</span><span class="sxs-lookup"><span data-stu-id="229b4-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="229b4-197">Zachowanie jest jednak takie samo, jeśli odwołanie od elementu zależnego/podrzędnego do podmiotu zabezpieczeń/nadrzędne jest w zamian wartością null.</span><span class="sxs-lookup"><span data-stu-id="229b4-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="229b4-198">Zapoznaj się z informacjami o tym, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="229b4-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="229b4-199">DeleteBehavior. Kaskada z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="229b4-200">Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="229b4-201">Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="229b4-202">Metody SaveChanges wysyła usunięcia dla elementów zależnych/podrzędnych (wpisów)</span><span class="sxs-lookup"><span data-stu-id="229b4-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="229b4-203">Po zapisaniu elementy zależne/podrzędne (wpisy) są odłączone, ponieważ zostały usunięte z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="229b4-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="229b4-204">DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z wymaganą relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="229b4-205">Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="229b4-206">Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="229b4-207">Metody SaveChanges próbuje ustawić wartość null dla wpisu klucza obcego, ale to nie powiodło się, ponieważ obcy nie dopuszcza wartości null</span><span class="sxs-lookup"><span data-stu-id="229b4-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="229b4-208">DeleteBehavior. ClientSetNull lub DeleteBehavior. SetNull z opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="229b4-209">Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="229b4-210">Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="229b4-211">Metody SaveChanges ustawia wartość klucza obcego obu zależności/elementów podrzędnych (wpisów) na null</span><span class="sxs-lookup"><span data-stu-id="229b4-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="229b4-212">Po zapisaniu elementy zależne/podrzędne (wpisy) mają teraz wartości klucza obcego i ich odwołanie do usuniętego podmiotu zabezpieczeń/nadrzędnego (blog) zostało usunięte</span><span class="sxs-lookup"><span data-stu-id="229b4-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="229b4-213">DeleteBehavior. Ogranicz z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="229b4-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="229b4-214">Wpisy są oznaczane jako zmodyfikowane, ponieważ powoduje to odłączenie relacji, co spowodowało oznaczenie klucza obcego jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="229b4-215">Jeśli obcy nie dopuszcza wartości null, wartość rzeczywista nie zmieni się, mimo że jest oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="229b4-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="229b4-216">Ponieważ wartość *ograniczenia* powoduje, że EF nie ustawia automatycznie klucza obcego na null, pozostaje nierówna null i metody SaveChanges zgłasza bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="229b4-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="229b4-217">Kaskadowanie do nieśledzonych jednostek</span><span class="sxs-lookup"><span data-stu-id="229b4-217">Cascading to untracked entities</span></span>

<span data-ttu-id="229b4-218">Po wywołaniu *metody SaveChanges*reguły usuwania kaskadowego będą stosowane do wszystkich jednostek, które są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="229b4-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="229b4-219">Jest to sytuacja we wszystkich przykładach przedstawionych powyżej, co oznacza, że program SQL został wygenerowany w celu usunięcia zarówno podmiotu zabezpieczeń, jak i wszystkich elementów zależnych/podrzędnych (wpisów):</span><span class="sxs-lookup"><span data-stu-id="229b4-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="229b4-220">W przypadku załadowania tylko podmiotu zabezpieczeń — na przykład gdy kwerenda zostanie wykonana dla blogu bez `Include(b => b.Posts)` do dołączenia wpisów, a następnie metody SaveChanges będzie generować tylko SQL w celu usunięcia podmiotu zabezpieczeń/elementu nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="229b4-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="229b4-221">Zależności/elementy podrzędne (wpisy) zostaną usunięte tylko wtedy, gdy baza danych ma skonfigurowane odpowiednie zachowanie kaskadowe.</span><span class="sxs-lookup"><span data-stu-id="229b4-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="229b4-222">Jeśli utworzysz bazę danych za pomocą programu EF, to zachowanie kaskadowe zostanie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="229b4-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
