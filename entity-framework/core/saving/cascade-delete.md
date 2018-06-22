---
title: Usuwanie - EF Core kaskadowe
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812680"
---
# <a name="cascade-delete"></a><span data-ttu-id="36894-102">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="36894-102">Cascade Delete</span></span>

<span data-ttu-id="36894-103">Usuwanie kaskadowe jest zwykle używany w terminologii bazy danych w przypadku cech, który umożliwia usunięcie wiersza automatycznie wyzwalać usunięcie powiązane wiersze.</span><span class="sxs-lookup"><span data-stu-id="36894-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="36894-104">Ściśle pojęciem objętych zachowania delete EF Core jest automatyczne usuwanie obiektu podrzędnego w przypadku relacji do elementu nadrzędnego zostały Przerwano — jest to często nazywane "Usuwanie oddzielone".</span><span class="sxs-lookup"><span data-stu-id="36894-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="36894-105">Podstawowe EF implementuje kilka różnych usuwania zachowań i umożliwia konfigurację zachowania delete poszczególnych relacji.</span><span class="sxs-lookup"><span data-stu-id="36894-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="36894-106">Podstawowe EF implementuje również automatycznie konfigurować domyślne przydatne usunięcie zachowania dla każdej relacji na podstawie Konwencji [requiredness relacji](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="36894-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="36894-107">Usuń zachowania</span><span class="sxs-lookup"><span data-stu-id="36894-107">Delete behaviors</span></span>
<span data-ttu-id="36894-108">Usuń zachowania są zdefiniowane w *DeleteBehavior* modułu wyliczającego wpisz i mogą zostać przekazane do *OnDelete* interfejsu API fluent do kontroli czy usunięcie jednostki nadrzędne podmiot zabezpieczeń lub severing programu Relacja podmioty zależne od/podrzędny musi mieć efektem ubocznym WE podmioty zależne od/podrzędny.</span><span class="sxs-lookup"><span data-stu-id="36894-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="36894-109">Istnieją trzy czynności, które EF można wykonać po usunięciu jednostki nadrzędne podmiot zabezpieczeń lub relacji do elementu podrzędnego jest Przerwano:</span><span class="sxs-lookup"><span data-stu-id="36894-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="36894-110">Podrzędne/zależnych mogą zostać usunięte.</span><span class="sxs-lookup"><span data-stu-id="36894-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="36894-111">Można ustawić wartości kluczy obcych dziecka na wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="36894-112">Obiekt podrzędny nie jest zmieniany</span><span class="sxs-lookup"><span data-stu-id="36894-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="36894-113">Zachowanie delete skonfigurowane w modelu podstawowej EF jest stosowany tylko wtedy, gdy główna jednostka zostanie usunięta za pomocą EF Core i podmioty zależne są ładowane do pamięci (np. do śledzonych zależnych).</span><span class="sxs-lookup"><span data-stu-id="36894-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="36894-114">Odpowiednie zachowanie cascade należy się, że ustawienia w bazie danych, aby upewnić się, dane, które nie jest śledzony przez kontekst ma niezbędnych działań, które są stosowane.</span><span class="sxs-lookup"><span data-stu-id="36894-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="36894-115">Jeśli używasz EF Core utworzyć bazę danych, to zachowanie w kaskadowego będzie Instalator automatycznie.</span><span class="sxs-lookup"><span data-stu-id="36894-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="36894-116">Dla drugiej akcji powyższe ustawienie wartości klucza obcego do wartości null jest nieprawidłowy w przypadku klucza obcego nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="36894-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="36894-117">(Wartości null klucz obcy jest odpowiednikiem wymaganej relacji). W takich przypadkach EF Core śledzi, że właściwość klucza obcego została oznaczona jako null aż po wywołaniu metody SaveChanges po tym czasie jest zwracany wyjątek, ponieważ zmiana nie może zostać utrwalona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="36894-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="36894-118">Jest to podobne do pobierania naruszenie ograniczenia z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36894-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="36894-119">Istnieją cztery usunąć zachowania, wymienione w poniższych tabelach.</span><span class="sxs-lookup"><span data-stu-id="36894-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="36894-120">W przypadku relacji opcjonalny (wartość null klucz obcy) on _jest_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:</span><span class="sxs-lookup"><span data-stu-id="36894-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="36894-121">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="36894-121">Behavior Name</span></span>               | <span data-ttu-id="36894-122">Wpływ na zależne od/podrzędny w pamięci</span><span class="sxs-lookup"><span data-stu-id="36894-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="36894-123">Wpływ na zależne od/podrzędny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="36894-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="36894-124">**Kaskadowo**</span><span class="sxs-lookup"><span data-stu-id="36894-124">**Cascade**</span></span>                 | <span data-ttu-id="36894-125">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="36894-125">Entities are deleted</span></span>                   | <span data-ttu-id="36894-126">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="36894-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="36894-127">**ClientSetNull** (domyślna)</span><span class="sxs-lookup"><span data-stu-id="36894-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="36894-128">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="36894-129">Brak</span><span class="sxs-lookup"><span data-stu-id="36894-129">None</span></span>                                   |
| <span data-ttu-id="36894-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="36894-130">**SetNull**</span></span>                 | <span data-ttu-id="36894-131">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="36894-132">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="36894-133">**Ogranicz**</span><span class="sxs-lookup"><span data-stu-id="36894-133">**Restrict**</span></span>                | <span data-ttu-id="36894-134">Brak</span><span class="sxs-lookup"><span data-stu-id="36894-134">None</span></span>                                   | <span data-ttu-id="36894-135">Brak</span><span class="sxs-lookup"><span data-stu-id="36894-135">None</span></span>                                   |

<span data-ttu-id="36894-136">Wymagane relacje (klucza obcego nie dopuszcza wartości null) jest _nie_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:</span><span class="sxs-lookup"><span data-stu-id="36894-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="36894-137">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="36894-137">Behavior Name</span></span>         | <span data-ttu-id="36894-138">Wpływ na zależne od/podrzędny w pamięci</span><span class="sxs-lookup"><span data-stu-id="36894-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="36894-139">Wpływ na zależne od/podrzędny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="36894-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="36894-140">**CASCADE** (domyślna)</span><span class="sxs-lookup"><span data-stu-id="36894-140">**Cascade** (Default)</span></span> | <span data-ttu-id="36894-141">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="36894-141">Entities are deleted</span></span>                | <span data-ttu-id="36894-142">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="36894-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="36894-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="36894-143">**ClientSetNull**</span></span>     | <span data-ttu-id="36894-144">Zgłasza SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-144">SaveChanges throws</span></span>                  | <span data-ttu-id="36894-145">Brak</span><span class="sxs-lookup"><span data-stu-id="36894-145">None</span></span>                                  |
| <span data-ttu-id="36894-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="36894-146">**SetNull**</span></span>           | <span data-ttu-id="36894-147">Zgłasza SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-147">SaveChanges throws</span></span>                  | <span data-ttu-id="36894-148">Zgłasza SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="36894-149">**Ogranicz**</span><span class="sxs-lookup"><span data-stu-id="36894-149">**Restrict**</span></span>          | <span data-ttu-id="36894-150">Brak</span><span class="sxs-lookup"><span data-stu-id="36894-150">None</span></span>                                | <span data-ttu-id="36894-151">Brak</span><span class="sxs-lookup"><span data-stu-id="36894-151">None</span></span>                                  |

<span data-ttu-id="36894-152">W tabelach powyżej *Brak* może spowodować naruszenie ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="36894-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="36894-153">Na przykład jeśli obiekt principal/podrzędny jest usuwany, ale nie podjęto żadnej akcji można zmienić klucza obcego zależne od/podrzędnego, następnie bazy danych prawdopodobnie zgłosi na SaveChanges z powodu naruszenia ograniczenia obcego.</span><span class="sxs-lookup"><span data-stu-id="36894-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="36894-154">Na wysokim poziomie:</span><span class="sxs-lookup"><span data-stu-id="36894-154">At a high level:</span></span>
* <span data-ttu-id="36894-155">Jeśli masz jednostek, które nie mogą istnieć bez klasy nadrzędnej, a EF automatyzującą automatycznie usuwania elementów podrzędnych, a następnie użyć *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="36894-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="36894-156">Jednostek, które nie mogą istnieć bez elementu nadrzędnego zwykle należy użyć wymagane relacji, dla którego *Cascade* jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="36894-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="36894-157">Jeśli masz jednostek, które mogą lub nie może mieć elementu nadrzędnego, a EF automatyzującą nulling się klucz obcy dla Ciebie, a następnie użyć *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="36894-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="36894-158">Jednostki, które może istnieć bez elementu nadrzędnego zwykle należy użyć relacji opcjonalne, dla którego *ClientSetNull* jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="36894-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="36894-159">Jeśli chcesz, aby bazę danych do też spróbować propagację wartości null do kluczy obcych podrzędnych nawet po obiektu podrzędnego nie został załadowany, następnie użyj *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="36894-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="36894-160">Jednak należy pamiętać, bazy danych musi obsługiwać to, czy Konfigurowanie bazy danych, takich jak to może spowodować inne ograniczenia, które w praktyce często sprawia, że ta opcja niepraktyczne.</span><span class="sxs-lookup"><span data-stu-id="36894-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="36894-161">Jest to dlaczego *SetNull* nie jest domyślnym.</span><span class="sxs-lookup"><span data-stu-id="36894-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="36894-162">Jeśli nie chcesz jądra EF kiedykolwiek automatycznie usuwać jednostki lub null limit klucza obcego automatycznie, a następnie użyj *Ogranicz*.</span><span class="sxs-lookup"><span data-stu-id="36894-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="36894-163">Uwaga to wymaga, że kod synchronizowania jednostek podrzędnych i ich wartości kluczy obcych ręcznie w przeciwnym razie ograniczenia wyjątki będą zgłaszane.</span><span class="sxs-lookup"><span data-stu-id="36894-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="36894-164">W podstawowej EF, w odróżnieniu od EF6 efekty kaskadowych odbywa się natychmiast, ale zamiast tego tylko wtedy, gdy jest wywoływana metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="36894-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="36894-165">**Zmiany w programie EF Core 2.0:** w poprzednich wersjach *Ogranicz* spowodowałoby opcjonalne właściwości klucza obcego w śledzonych jednostek zależnych, należy ustawić wartości null i został domyślnie Usuń zachowanie w przypadku relacji opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="36894-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="36894-166">W programie EF Core 2.0 *ClientSetNull* wprowadzono w celu reprezentowania tego zachowania i stał się wartością domyślną opcjonalne relacji.</span><span class="sxs-lookup"><span data-stu-id="36894-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="36894-167">Zachowanie *Ogranicz* została dostosowana do nigdy nie ma żadnych efektów ubocznych na jednostek zależnych.</span><span class="sxs-lookup"><span data-stu-id="36894-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="36894-168">Przykłady usunięcie jednostki</span><span class="sxs-lookup"><span data-stu-id="36894-168">Entity deletion examples</span></span>

<span data-ttu-id="36894-169">Poniższy kod jest częścią [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) który można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="36894-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="36894-170">Przykład pokazuje, co się stanie dla każdego zachowanie Usuń relacje zarówno opcjonalne i wymagane po usunięciu jednostki nadrzędnej.</span><span class="sxs-lookup"><span data-stu-id="36894-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="36894-171">Przejdźmy każdej zmiany, aby zrozumieć, co dzieje się.</span><span class="sxs-lookup"><span data-stu-id="36894-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="36894-172">DeleteBehavior.Cascade z relacją wymagany lub opcjonalny</span><span class="sxs-lookup"><span data-stu-id="36894-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="36894-173">Blog jest oznaczone jako usunięte</span><span class="sxs-lookup"><span data-stu-id="36894-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="36894-174">Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="36894-175">Metody SaveChanges wysyła usuwa zarówno zależności/elementy podrzędne (ogłoszeń), a następnie principal/nadrzędnego (blog)</span><span class="sxs-lookup"><span data-stu-id="36894-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="36894-176">Po zapisaniu wszystkich jednostek są odłączone od teraz zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="36894-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="36894-177">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relację</span><span class="sxs-lookup"><span data-stu-id="36894-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="36894-178">Blog jest oznaczone jako usunięte</span><span class="sxs-lookup"><span data-stu-id="36894-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="36894-179">Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="36894-180">Metody SaveChanges próbuje ustawić po klucz OBCY na wartość null, ale to nie działa, ponieważ klucza Obcego nie dopuszcza wartości null</span><span class="sxs-lookup"><span data-stu-id="36894-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="36894-181">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relację</span><span class="sxs-lookup"><span data-stu-id="36894-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="36894-182">Blog jest oznaczone jako usunięte</span><span class="sxs-lookup"><span data-stu-id="36894-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="36894-183">Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="36894-184">Metody SaveChanges prób Ustawia klucz OBCY zarówno zależności/podrzędnych (ogłoszeń) na wartość null przed usunięciem principal/nadrzędnego (blog)</span><span class="sxs-lookup"><span data-stu-id="36894-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="36894-185">Po zapisaniu principal/nadrzędnego (blog) zostanie usunięty, ale zależności/podrzędnych (ogłoszeń) nadal są śledzone.</span><span class="sxs-lookup"><span data-stu-id="36894-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="36894-186">Śledzonych zależności/podrzędnych (ogłoszeń) teraz mieć wartości null klucza Obcego i ich odwołanie do usuniętego principal/nadrzędnego (blog) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="36894-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="36894-187">DeleteBehavior.Restrict z relacją wymagany lub opcjonalny</span><span class="sxs-lookup"><span data-stu-id="36894-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="36894-188">Blog jest oznaczone jako usunięte</span><span class="sxs-lookup"><span data-stu-id="36894-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="36894-189">Wpisy początkowo pozostać niezmieniony, ponieważ kaskady odbywa się do metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="36894-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="36894-190">Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawiony na wartość null, klucza Obcego pozostaje inną niż null i zgłasza SaveChanges bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="36894-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="36894-191">Usuń porzucone przykłady</span><span class="sxs-lookup"><span data-stu-id="36894-191">Delete orphans examples</span></span>

<span data-ttu-id="36894-192">Poniższy kod jest częścią [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) który można pobrać uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="36894-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="36894-193">Próbka przedstawiono dla każdego zachowanie Usuń relacje zarówno opcjonalne i wymagane jest Przerwano relacji nadrzędny/podmiot zabezpieczeń i jego elementy podrzędne/zależności.</span><span class="sxs-lookup"><span data-stu-id="36894-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="36894-194">W tym przykładzie relacji jest Przerwano przez usunięcie zależności/podrzędnych (ogłoszeń) z właściwością nawigacji kolekcji principal/nadrzędnego (blog).</span><span class="sxs-lookup"><span data-stu-id="36894-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="36894-195">Jednak zachowanie jest takie samo Jeśli odwołania z potomnym zależne od podmiotu/Parent jest zamiast tego pustych wychodzących.</span><span class="sxs-lookup"><span data-stu-id="36894-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="36894-196">Przejdźmy każdej zmiany, aby zrozumieć, co dzieje się.</span><span class="sxs-lookup"><span data-stu-id="36894-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="36894-197">DeleteBehavior.Cascade z relacją wymagany lub opcjonalny</span><span class="sxs-lookup"><span data-stu-id="36894-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="36894-198">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="36894-199">Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="36894-200">Metody SaveChanges wysyła usuwa zależności/podrzędnych (ogłoszeń)</span><span class="sxs-lookup"><span data-stu-id="36894-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="36894-201">Po zapisaniu zależności/podrzędnych (ogłoszeń) są odłączone od teraz zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="36894-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="36894-202">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relację</span><span class="sxs-lookup"><span data-stu-id="36894-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="36894-203">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="36894-204">Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="36894-205">Metody SaveChanges próbuje ustawić po klucz OBCY na wartość null, ale to nie działa, ponieważ klucza Obcego nie dopuszcza wartości null</span><span class="sxs-lookup"><span data-stu-id="36894-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="36894-206">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relację</span><span class="sxs-lookup"><span data-stu-id="36894-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="36894-207">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="36894-208">Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="36894-209">Metody SaveChanges Ustawia klucz OBCY zarówno zależności/podrzędnych (ogłoszeń) na wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="36894-210">Po zapisaniu zależności/podrzędnych (ogłoszeń) teraz mieć wartości null klucza Obcego i ich odwołanie do usuniętego principal/nadrzędnego (blog) zostały usunięte.</span><span class="sxs-lookup"><span data-stu-id="36894-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="36894-211">DeleteBehavior.Restrict z relacją wymagany lub opcjonalny</span><span class="sxs-lookup"><span data-stu-id="36894-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="36894-212">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego może być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="36894-213">Jeśli klucza Obcego nie dopuszcza wartości null, następnie rzeczywistej wartości nie spowoduje zmiany, nawet jeśli jest oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="36894-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="36894-214">Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawiony na wartość null, klucza Obcego pozostaje inną niż null i zgłasza SaveChanges bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="36894-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="36894-215">Kaskadowa dla nieśledzonych jednostek</span><span class="sxs-lookup"><span data-stu-id="36894-215">Cascading to untracked entities</span></span>

<span data-ttu-id="36894-216">Podczas wywoływania *SaveChanges*, cascade Usuń zasady zostaną zastosowane do żadnych jednostek, które są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="36894-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="36894-217">Jest to sytuacja we wszystkich przykładach przedstawionych powyżej, tworzonym SQL został wygenerowany, aby usunąć zarówno principal/nadrzędnego (blog) oraz zależności/elementy podrzędne (ogłoszeń):</span><span class="sxs-lookup"><span data-stu-id="36894-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="36894-218">Jeśli podmiot zabezpieczeń jest załadowany — tylko na przykład kwerendy nawiązaniem blogu bez `Include(b => b.Posts)` aby obejmować wpisów — następnie SaveChanges wygeneruje jedynie SQL, aby usunąć nadrzędne podmiot zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="36894-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="36894-219">Zależności/podrzędnych (ogłoszeń) zostanie usunięte tylko w przypadku, gdy baza danych ma skonfigurowane odpowiednie zachowanie cascade.</span><span class="sxs-lookup"><span data-stu-id="36894-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="36894-220">Jeśli używasz EF utworzyć bazę danych, to zachowanie w kaskadowego będzie Instalator automatycznie.</span><span class="sxs-lookup"><span data-stu-id="36894-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
