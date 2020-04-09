---
title: Kaskadowe usuwanie - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417615"
---
# <a name="cascade-delete"></a><span data-ttu-id="68c2a-102">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="68c2a-102">Cascade Delete</span></span>

<span data-ttu-id="68c2a-103">Kaskadowe usuwanie jest często używane w terminologii bazy danych do opisania cechy, która umożliwia usunięcie wiersza, aby automatycznie wyzwolić usunięcie powiązanych wierszy.</span><span class="sxs-lookup"><span data-stu-id="68c2a-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="68c2a-104">Ściśle powiązaną koncepcją również objęte ef core usuwania zachowań jest automatyczne usunięcie jednostki podrzędnej, gdy jest to relacja z elementem nadrzędnym został odcięty - jest to powszechnie znane jako "usuwanie sierot".</span><span class="sxs-lookup"><span data-stu-id="68c2a-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="68c2a-105">EF Core implementuje kilka różnych zachowań usuwania i umożliwia konfigurację zachowań usuwania poszczególnych relacji.</span><span class="sxs-lookup"><span data-stu-id="68c2a-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="68c2a-106">EF Core implementuje również konwencje, które automatycznie konfigurują przydatne domyślne zachowania usuwania dla każdej relacji na podstawie [wymaganej relacji](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="68c2a-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="68c2a-107">Usuwanie zachowań</span><span class="sxs-lookup"><span data-stu-id="68c2a-107">Delete behaviors</span></span>

<span data-ttu-id="68c2a-108">Zachowania delete są zdefiniowane w *DeleteBehavior* typ wyliczania i mogą być przekazywane do *OnDelete płynnie* interfejsu API do kontrolowania, czy usunięcie jednostki podmiotu głównego/nadrzędnego lub zerwanie relacji do jednostek zależnych/podrzędnych powinny mieć wpływ uboczny na jednostki zależne/podrzędne.</span><span class="sxs-lookup"><span data-stu-id="68c2a-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="68c2a-109">Istnieją trzy akcje EF można podjąć, gdy jednostka podmiotu głównego/nadrzędnego jest usuwany lub relacji z elementami podrzędnymi jest zerwana:</span><span class="sxs-lookup"><span data-stu-id="68c2a-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="68c2a-110">Dziecko/pozostające na utrzymaniu można usunąć</span><span class="sxs-lookup"><span data-stu-id="68c2a-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="68c2a-111">Wartości klucza obcego dziecka można ustawić na wartość null</span><span class="sxs-lookup"><span data-stu-id="68c2a-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="68c2a-112">Dziecko pozostaje niezmienione</span><span class="sxs-lookup"><span data-stu-id="68c2a-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="68c2a-113">Zachowanie usuwania skonfigurowane w modelu EF Core jest stosowane tylko wtedy, gdy jednostka główna jest usuwana przy użyciu EF Core i encje zależne są ładowane do pamięci (czyli dla śledzonych zależności).</span><span class="sxs-lookup"><span data-stu-id="68c2a-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="68c2a-114">Odpowiednie zachowanie kaskadowe musi być skonfigurowane w bazie danych, aby upewnić się, że dane, które nie są śledzone przez kontekst, mają zastosowaną niezbędną akcję.</span><span class="sxs-lookup"><span data-stu-id="68c2a-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="68c2a-115">Jeśli używasz EF Core do utworzenia bazy danych, to zachowanie kaskadowe zostanie skonfigurowane dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="68c2a-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="68c2a-116">W przypadku drugiej powyższej akcji ustawienie wartości klucza obcego na null jest nieprawidłowe, jeśli klucz obcy nie jest nullable.</span><span class="sxs-lookup"><span data-stu-id="68c2a-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="68c2a-117">(Klucz obcy nienastępny do null jest odpowiednikiem wymaganej relacji). W takich przypadkach EF Core śledzi, że właściwość klucza obcego została oznaczona jako null, dopóki SaveChanges jest wywoływana, w którym czasie wyjątek jest zgłaszany, ponieważ zmiana nie może być utrwalone do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68c2a-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="68c2a-118">Jest to podobne do uzyskania naruszenia ograniczeń z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68c2a-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="68c2a-119">Istnieją cztery zachowania usuwania, wymienione w poniższych tabelach.</span><span class="sxs-lookup"><span data-stu-id="68c2a-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="68c2a-120">Opcjonalne relacje</span><span class="sxs-lookup"><span data-stu-id="68c2a-120">Optional relationships</span></span>

<span data-ttu-id="68c2a-121">W przypadku relacji opcjonalnych (nullable foreign key) możliwe _jest_ zapisanie wartości klucza obcego null, co powoduje następujące efekty:</span><span class="sxs-lookup"><span data-stu-id="68c2a-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="68c2a-122">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="68c2a-122">Behavior Name</span></span>               | <span data-ttu-id="68c2a-123">Wpływ na zależne/dziecko w pamięci</span><span class="sxs-lookup"><span data-stu-id="68c2a-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="68c2a-124">Wpływ na zależne/podrzędne w bazie danych</span><span class="sxs-lookup"><span data-stu-id="68c2a-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="68c2a-125">**Kaskadowo**</span><span class="sxs-lookup"><span data-stu-id="68c2a-125">**Cascade**</span></span>                 | <span data-ttu-id="68c2a-126">Jednostki są usuwane</span><span class="sxs-lookup"><span data-stu-id="68c2a-126">Entities are deleted</span></span>                   | <span data-ttu-id="68c2a-127">Jednostki są usuwane</span><span class="sxs-lookup"><span data-stu-id="68c2a-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="68c2a-128">**ClientSetNull** (domyślnie)</span><span class="sxs-lookup"><span data-stu-id="68c2a-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="68c2a-129">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="68c2a-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="68c2a-130">Brak</span><span class="sxs-lookup"><span data-stu-id="68c2a-130">None</span></span>                                   |
| <span data-ttu-id="68c2a-131">**Setnull**</span><span class="sxs-lookup"><span data-stu-id="68c2a-131">**SetNull**</span></span>                 | <span data-ttu-id="68c2a-132">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="68c2a-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="68c2a-133">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="68c2a-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="68c2a-134">**Ograniczyć**</span><span class="sxs-lookup"><span data-stu-id="68c2a-134">**Restrict**</span></span>                | <span data-ttu-id="68c2a-135">Brak</span><span class="sxs-lookup"><span data-stu-id="68c2a-135">None</span></span>                                   | <span data-ttu-id="68c2a-136">Brak</span><span class="sxs-lookup"><span data-stu-id="68c2a-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="68c2a-137">Wymagane relacje</span><span class="sxs-lookup"><span data-stu-id="68c2a-137">Required relationships</span></span>

<span data-ttu-id="68c2a-138">Dla wymaganych relacji (klucz obcy nienastępny do null) _nie_ jest możliwe zapisanie wartości klucza obcego null, co powoduje następujące efekty:</span><span class="sxs-lookup"><span data-stu-id="68c2a-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="68c2a-139">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="68c2a-139">Behavior Name</span></span>         | <span data-ttu-id="68c2a-140">Wpływ na zależne/dziecko w pamięci</span><span class="sxs-lookup"><span data-stu-id="68c2a-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="68c2a-141">Wpływ na zależne/podrzędne w bazie danych</span><span class="sxs-lookup"><span data-stu-id="68c2a-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="68c2a-142">**Kaskada** (domyślna)</span><span class="sxs-lookup"><span data-stu-id="68c2a-142">**Cascade** (Default)</span></span> | <span data-ttu-id="68c2a-143">Jednostki są usuwane</span><span class="sxs-lookup"><span data-stu-id="68c2a-143">Entities are deleted</span></span>                | <span data-ttu-id="68c2a-144">Jednostki są usuwane</span><span class="sxs-lookup"><span data-stu-id="68c2a-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="68c2a-145">**KlientSetNull**</span><span class="sxs-lookup"><span data-stu-id="68c2a-145">**ClientSetNull**</span></span>     | <span data-ttu-id="68c2a-146">SaveChanges rzuca</span><span class="sxs-lookup"><span data-stu-id="68c2a-146">SaveChanges throws</span></span>                  | <span data-ttu-id="68c2a-147">Brak</span><span class="sxs-lookup"><span data-stu-id="68c2a-147">None</span></span>                                  |
| <span data-ttu-id="68c2a-148">**Setnull**</span><span class="sxs-lookup"><span data-stu-id="68c2a-148">**SetNull**</span></span>           | <span data-ttu-id="68c2a-149">SaveChanges rzuca</span><span class="sxs-lookup"><span data-stu-id="68c2a-149">SaveChanges throws</span></span>                  | <span data-ttu-id="68c2a-150">SaveChanges rzuca</span><span class="sxs-lookup"><span data-stu-id="68c2a-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="68c2a-151">**Ograniczyć**</span><span class="sxs-lookup"><span data-stu-id="68c2a-151">**Restrict**</span></span>          | <span data-ttu-id="68c2a-152">Brak</span><span class="sxs-lookup"><span data-stu-id="68c2a-152">None</span></span>                                | <span data-ttu-id="68c2a-153">Brak</span><span class="sxs-lookup"><span data-stu-id="68c2a-153">None</span></span>                                  |

<span data-ttu-id="68c2a-154">W tabelach powyżej *None* może spowodować naruszenie ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="68c2a-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="68c2a-155">Na przykład jeśli jednostka podmiotu dublownik/dziecko zostanie usunięta, ale nie zostanie podjęta żadna akcja w celu zmiany klucza obcego zależności/dziecka, baza danych prawdopodobnie zostanie usunięta na SaveChanges z powodu naruszenia ograniczeń zagranicznych.</span><span class="sxs-lookup"><span data-stu-id="68c2a-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="68c2a-156">Na wysokim poziomie:</span><span class="sxs-lookup"><span data-stu-id="68c2a-156">At a high level:</span></span>

* <span data-ttu-id="68c2a-157">Jeśli masz encje, które nie mogą istnieć bez nadrzędnego i chcesz, aby EF zajmował się automatycznym usuwaniem elementów podrzędnych, użyj *kaskadowego*.</span><span class="sxs-lookup"><span data-stu-id="68c2a-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="68c2a-158">Jednostki, które nie mogą istnieć bez jednostki nadrzędnej zwykle korzystają z wymaganych relacji, dla których *Kaskada* jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="68c2a-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="68c2a-159">Jeśli masz jednostki, które mogą lub nie mogą mieć nadrzędnego, i chcesz EF dbać o unieważnienie klucza obcego dla Ciebie, a następnie użyć *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="68c2a-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="68c2a-160">Jednostki, które mogą istnieć bez jednostki nadrzędnej zwykle korzystają z opcjonalnych relacji, dla których *ClientSetNull* jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="68c2a-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="68c2a-161">Jeśli chcesz, aby baza danych próbowała również propagować wartości null do podrzędnych kluczy obcych, nawet jeśli jednostka podrzędna nie jest załadowana, użyj *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="68c2a-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="68c2a-162">Należy jednak pamiętać, że baza danych musi obsługiwać to i konfigurowanie bazy danych w ten sposób może spowodować inne ograniczenia, które w praktyce często sprawia, że ta opcja niepraktyczne.</span><span class="sxs-lookup"><span data-stu-id="68c2a-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="68c2a-163">Dlatego *SetNull* nie jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="68c2a-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="68c2a-164">Jeśli nie chcesz, aby EF Core kiedykolwiek automatycznie usuwał encję lub automatycznie wyzerowywał klucz obcy, użyj opcji *Ogranicz*.</span><span class="sxs-lookup"><span data-stu-id="68c2a-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="68c2a-165">Należy zauważyć, że wymaga to, aby kod przechowywać jednostki podrzędne i ich wartości klucza obcego w synchronizacji ręcznie w przeciwnym razie wyjątki ograniczenia zostaną odrzucone.</span><span class="sxs-lookup"><span data-stu-id="68c2a-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="68c2a-166">W EF Core, w przeciwieństwie do EF6, efekty kaskadowe nie zdarzają się natychmiast, ale zamiast tego tylko wtedy, gdy savechanges jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="68c2a-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="68c2a-167">**Zmiany w EF Core 2.0:** W poprzednich wersjach *Restrict* spowodowałoby opcjonalne właściwości klucza obcego w śledzonych jednostek zależnych, które mają być ustawione na wartość null i było domyślne zachowanie usuwania dla relacji opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="68c2a-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="68c2a-168">W EF Core 2.0 *ClientSetNull* został wprowadzony do reprezentowania tego zachowania i stał się domyślny dla relacji opcjonalnych.</span><span class="sxs-lookup"><span data-stu-id="68c2a-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="68c2a-169">Zachowanie *Restrict* został dostosowany do nigdy nie mają żadnych skutków ubocznych na jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="68c2a-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="68c2a-170">Przykłady usuwania encji</span><span class="sxs-lookup"><span data-stu-id="68c2a-170">Entity deletion examples</span></span>

<span data-ttu-id="68c2a-171">Poniższy kod jest częścią [próbki,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) które można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="68c2a-171">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="68c2a-172">Przykład pokazuje, co się dzieje dla każdego zachowania usuwania dla relacji opcjonalnych i wymaganych, gdy jednostka nadrzędna jest usuwana.</span><span class="sxs-lookup"><span data-stu-id="68c2a-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="68c2a-173">Przejdźmy przez każdą odmianę, aby zrozumieć, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="68c2a-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="68c2a-174">UsuńBehavior.Kaskada z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="68c2a-175">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="68c2a-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="68c2a-176">Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges</span><span class="sxs-lookup"><span data-stu-id="68c2a-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="68c2a-177">SaveChanges wysyła usuwa zarówno na utrzymaniu /dzieci (posty), a następnie główny/nadrzędny (blog)</span><span class="sxs-lookup"><span data-stu-id="68c2a-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="68c2a-178">Po zapisaniu wszystkie jednostki są odłączone, ponieważ zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="68c2a-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="68c2a-179">UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="68c2a-180">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="68c2a-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="68c2a-181">Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges</span><span class="sxs-lookup"><span data-stu-id="68c2a-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="68c2a-182">SaveChanges próbuje ustawić wpis FK na null, ale to nie powiedzie się, ponieważ FK nie jest nullable</span><span class="sxs-lookup"><span data-stu-id="68c2a-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="68c2a-183">UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="68c2a-184">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="68c2a-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="68c2a-185">Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges</span><span class="sxs-lookup"><span data-stu-id="68c2a-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="68c2a-186">SaveChanges próbuje ustawia FK obu utrzymaniu /podstawowych (postów) do wartości null przed usunięciem głównego/nadrzędnego (blog)</span><span class="sxs-lookup"><span data-stu-id="68c2a-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="68c2a-187">Po zapisaniu główny/nadrzędny (blog) jest usuwany, ale osoby pozostające na utrzymaniu/dzieci (posty) są nadal śledzone</span><span class="sxs-lookup"><span data-stu-id="68c2a-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="68c2a-188">Śledzone osoby zależne/podrzędne (posty) mają teraz wartości null FK, a ich odwołanie do usuniętego głównego zobowiązanego/nadrzędnego (bloga) zostało usunięte</span><span class="sxs-lookup"><span data-stu-id="68c2a-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="68c2a-189">UsuńBehavior.Ogranicz z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="68c2a-190">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="68c2a-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="68c2a-191">Posty początkowo pozostają niezmienione, ponieważ kaskady nie zdarzają się do czasu zmiany SaveChanges</span><span class="sxs-lookup"><span data-stu-id="68c2a-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="68c2a-192">Ponieważ *Restrict* nakazuje EF, aby nie ustawiał automatycznie wartości null, pozostaje bez wartości null, a SaveChanges rzuca bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="68c2a-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="68c2a-193">Usuwanie przykładów sierot</span><span class="sxs-lookup"><span data-stu-id="68c2a-193">Delete orphans examples</span></span>

<span data-ttu-id="68c2a-194">Poniższy kod jest częścią [próbki,](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) które można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="68c2a-194">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="68c2a-195">Przykład pokazuje, co się dzieje dla każdego zachowania usuwania dla relacji opcjonalnych i wymaganych, gdy relacja między nadrzędnym/głównym i jego podstawowych/zależnych jest zerwana.</span><span class="sxs-lookup"><span data-stu-id="68c2a-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="68c2a-196">W tym przykładzie relacja jest zerwana przez usunięcie osób zależnych/elementów podrzędnych (wpisów) z właściwości nawigacji kolekcji w głównym/nadrzędnym (blogu).</span><span class="sxs-lookup"><span data-stu-id="68c2a-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="68c2a-197">Jednak zachowanie jest taka sama, jeśli odwołanie od zależne/podrzędne do głównego zobowiązanego/nadrzędnego jest zamiast tego anulowane.</span><span class="sxs-lookup"><span data-stu-id="68c2a-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="68c2a-198">Przejdźmy przez każdą odmianę, aby zrozumieć, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="68c2a-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="68c2a-199">UsuńBehavior.Kaskada z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="68c2a-200">Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="68c2a-201">Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="68c2a-202">SaveChanges wysyła usuwa dla osób pozostających na utrzymaniu /dzieci (posty)</span><span class="sxs-lookup"><span data-stu-id="68c2a-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="68c2a-203">Po zapisaniu osoby pozostające na utrzymaniu/dzieci (wpisy) są odłączone, ponieważ zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="68c2a-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="68c2a-204">UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="68c2a-205">Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="68c2a-206">Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="68c2a-207">SaveChanges próbuje ustawić wpis FK na null, ale to nie powiedzie się, ponieważ FK nie jest nullable</span><span class="sxs-lookup"><span data-stu-id="68c2a-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="68c2a-208">UsuńBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="68c2a-209">Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="68c2a-210">Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="68c2a-211">SaveChanges ustawia FK obu osób pozostających na utrzymaniu/dzieci (postów) na wartość null</span><span class="sxs-lookup"><span data-stu-id="68c2a-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="68c2a-212">Po zapisaniu osoby pozostające na utrzymaniu/dzieci (posty) mają teraz wartości null FK, a ich odwołanie do usuniętego głównego zobowiązanego/nadrzędnego (bloga) zostało usunięte</span><span class="sxs-lookup"><span data-stu-id="68c2a-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="68c2a-213">UsuńBehavior.Ogranicz z wymaganą lub opcjonalną relacją</span><span class="sxs-lookup"><span data-stu-id="68c2a-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="68c2a-214">Wpisy są oznaczone jako Zmodyfikowane, ponieważ zerwanie relacji spowodowało, że FK została oznaczona jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="68c2a-215">Jeśli FK nie jest nullable, a następnie rzeczywista wartość nie zmieni się, nawet jeśli jest oznaczony jako null</span><span class="sxs-lookup"><span data-stu-id="68c2a-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="68c2a-216">Ponieważ *Restrict* nakazuje EF, aby nie ustawiał automatycznie wartości null, pozostaje bez wartości null, a SaveChanges rzuca bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="68c2a-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="68c2a-217">Kaskadowe do nieśledzonych encji</span><span class="sxs-lookup"><span data-stu-id="68c2a-217">Cascading to untracked entities</span></span>

<span data-ttu-id="68c2a-218">Podczas *wywoływania SaveChanges*reguły usuwania kaskadowego zostaną zastosowane do wszystkich jednostek, które są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="68c2a-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="68c2a-219">Jest to sytuacja we wszystkich przykładach pokazanych powyżej, dlatego SQL został wygenerowany, aby usunąć zarówno głównego/nadrzędnego (blog) i wszystkich osób zależnych / elementów podrzędnych (postów):</span><span class="sxs-lookup"><span data-stu-id="68c2a-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="68c2a-220">Jeśli tylko podmiot zabezpieczeń jest załadowany - na przykład, gdy `Include(b => b.Posts)` kwerenda jest dla bloga bez również wpisy - a następnie SaveChanges będzie generować tylko SQL, aby usunąć głównego/nadrzędnego:</span><span class="sxs-lookup"><span data-stu-id="68c2a-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="68c2a-221">Osoby pozostające na utrzymaniu/dzieci (wpisy) zostaną usunięte tylko wtedy, gdy baza danych ma skonfigurowane odpowiednie zachowanie kaskadowe.</span><span class="sxs-lookup"><span data-stu-id="68c2a-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="68c2a-222">Jeśli do utworzenia bazy danych jest używana funkcja EF, to zachowanie kaskadowe zostanie skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="68c2a-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
