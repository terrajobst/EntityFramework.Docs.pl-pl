---
title: Usuwanie — EF Core kaskadowe
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: afe00ddb1b487c6b1b2ea42708c9967a57cea04b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995245"
---
# <a name="cascade-delete"></a><span data-ttu-id="f256b-102">Usuwanie kaskadowe</span><span class="sxs-lookup"><span data-stu-id="f256b-102">Cascade Delete</span></span>

<span data-ttu-id="f256b-103">Usuwanie kaskadowe jest najczęściej używany w terminologii bazy danych do opisania cechę, która umożliwia usunięcie wiersza automatycznie wyzwalać usunięcie powiązane wiersze.</span><span class="sxs-lookup"><span data-stu-id="f256b-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="f256b-104">Ściśle powiązanych koncepcji, również pasuje do żadnego zachowań usuwania programu EF Core jest automatyczne usuwanie obiektu podrzędnego, gdy relacji do elementu nadrzędnego zostały oddzielone — jest to często nazywane "Usuwanie porzucone".</span><span class="sxs-lookup"><span data-stu-id="f256b-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="f256b-105">EF Core implementuje kilka różnych delete zachowań i umożliwia konfigurację zachowania Usuń poszczególne relacji.</span><span class="sxs-lookup"><span data-stu-id="f256b-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="f256b-106">EF Core implementuje również konwencje, które automatycznie konfigurują zachowania delete przydatne domyślne dla każdej relacji na podstawie [requiredness relacji](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="f256b-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="f256b-107">Usuń zachowania</span><span class="sxs-lookup"><span data-stu-id="f256b-107">Delete behaviors</span></span>
<span data-ttu-id="f256b-108">Usuń zachowania są zdefiniowane w *DeleteBehavior* moduł wyliczający typu i mogą być przekazywane do *OnDelete* wygodnego interfejsu API do kontroli czy usunięcie jednostki jednostki/element nadrzędny lub severing programu Relacja do podmiotów zależnych od ustawień lokalnych/podrzędny musi mieć efekt uboczny w jednostek zależnych od ustawień lokalnych/podrzędny.</span><span class="sxs-lookup"><span data-stu-id="f256b-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="f256b-109">Dostępne są trzy akcje, które EF można podjąć, gdy jednostka jednostki/nadrzędna została usunięta lub jest oddzielone relacji do elementu podrzędnego:</span><span class="sxs-lookup"><span data-stu-id="f256b-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="f256b-110">Można je usunąć podrzędnych/zależnych od ustawień lokalnych</span><span class="sxs-lookup"><span data-stu-id="f256b-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="f256b-111">Wartości klucza obcego elementu podrzędnego można ustawić na wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="f256b-112">Element podrzędny nie jest zmieniany</span><span class="sxs-lookup"><span data-stu-id="f256b-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="f256b-113">Zachowanie dotyczące usuwania konfigurowane w modelu platformy EF Core jest stosowane tylko jednostki głównej jest usuwana za pomocą programu EF Core i podmioty zależne są ładowane do pamięci (czyli pod kątem śledzonych zależności).</span><span class="sxs-lookup"><span data-stu-id="f256b-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="f256b-114">Odpowiednie zachowania kaskadowe musi być Instalator w bazie danych, aby upewnić się, dane, które nie jest śledzony przez kontekst ma niezbędnych działań, które są stosowane.</span><span class="sxs-lookup"><span data-stu-id="f256b-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="f256b-115">Jeśli użyjesz programu EF Core do utworzenia bazy danych tego zachowania kaskadowe będzie Instalatora dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="f256b-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="f256b-116">Dla drugiego z powyższej akcji ustawienie wartości klucza obcego o wartości null jest nieprawidłowa w przypadku klucza obcego nie dopuszcza wartości null.</span><span class="sxs-lookup"><span data-stu-id="f256b-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="f256b-117">(Dopuszcza klucz obcy jest odpowiednikiem wymaganej relacji). W takich przypadkach programu EF Core śledzi, czy właściwość klucza obcego została oznaczona jako wartości null do momentu SaveChanges jest wywoływana, co jest zgłaszany wyjątek, ponieważ zmiana nie może zostać utrwalona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f256b-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="f256b-118">Jest to podobne do pobierania naruszenie ograniczenia z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f256b-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="f256b-119">Istnieją cztery Usuń zachowań, zgodnie z opisem w poniższych tabelach.</span><span class="sxs-lookup"><span data-stu-id="f256b-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="f256b-120">Opcjonalne relacji</span><span class="sxs-lookup"><span data-stu-id="f256b-120">Optional relationships</span></span>
<span data-ttu-id="f256b-121">W przypadku relacji opcjonalne (dopuszcza wartości null z kluczem obcym) on _jest_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:</span><span class="sxs-lookup"><span data-stu-id="f256b-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="f256b-122">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="f256b-122">Behavior Name</span></span>               | <span data-ttu-id="f256b-123">Wpływ na zależnych od ustawień lokalnych/podrzędny w pamięci</span><span class="sxs-lookup"><span data-stu-id="f256b-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="f256b-124">Wpływ na zależnych od ustawień lokalnych/podrzędnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="f256b-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="f256b-125">**Kaskadowe**</span><span class="sxs-lookup"><span data-stu-id="f256b-125">**Cascade**</span></span>                 | <span data-ttu-id="f256b-126">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="f256b-126">Entities are deleted</span></span>                   | <span data-ttu-id="f256b-127">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="f256b-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="f256b-128">**ClientSetNull** (opcja domyślna)</span><span class="sxs-lookup"><span data-stu-id="f256b-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="f256b-129">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="f256b-130">Brak</span><span class="sxs-lookup"><span data-stu-id="f256b-130">None</span></span>                                   |
| <span data-ttu-id="f256b-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="f256b-131">**SetNull**</span></span>                 | <span data-ttu-id="f256b-132">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="f256b-133">Właściwości klucza obcego są ustawione na wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="f256b-134">**ograniczenia**</span><span class="sxs-lookup"><span data-stu-id="f256b-134">**Restrict**</span></span>                | <span data-ttu-id="f256b-135">Brak</span><span class="sxs-lookup"><span data-stu-id="f256b-135">None</span></span>                                   | <span data-ttu-id="f256b-136">Brak</span><span class="sxs-lookup"><span data-stu-id="f256b-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="f256b-137">Wymagane relacje</span><span class="sxs-lookup"><span data-stu-id="f256b-137">Required relationships</span></span>
<span data-ttu-id="f256b-138">W przypadku relacji (innych niż null z kluczem obcym), wymagane jest _nie_ można zapisać wartości null wartości klucza obcego, co powoduje następujące skutki:</span><span class="sxs-lookup"><span data-stu-id="f256b-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="f256b-139">Nazwa zachowania</span><span class="sxs-lookup"><span data-stu-id="f256b-139">Behavior Name</span></span>         | <span data-ttu-id="f256b-140">Wpływ na zależnych od ustawień lokalnych/podrzędny w pamięci</span><span class="sxs-lookup"><span data-stu-id="f256b-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="f256b-141">Wpływ na zależnych od ustawień lokalnych/podrzędnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="f256b-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="f256b-142">**Kaskadowe** (opcja domyślna)</span><span class="sxs-lookup"><span data-stu-id="f256b-142">**Cascade** (Default)</span></span> | <span data-ttu-id="f256b-143">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="f256b-143">Entities are deleted</span></span>                | <span data-ttu-id="f256b-144">Jednostki są usuwane.</span><span class="sxs-lookup"><span data-stu-id="f256b-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="f256b-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="f256b-145">**ClientSetNull**</span></span>     | <span data-ttu-id="f256b-146">Zgłasza SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-146">SaveChanges throws</span></span>                  | <span data-ttu-id="f256b-147">Brak</span><span class="sxs-lookup"><span data-stu-id="f256b-147">None</span></span>                                  |
| <span data-ttu-id="f256b-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="f256b-148">**SetNull**</span></span>           | <span data-ttu-id="f256b-149">Zgłasza SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-149">SaveChanges throws</span></span>                  | <span data-ttu-id="f256b-150">Zgłasza SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="f256b-151">**ograniczenia**</span><span class="sxs-lookup"><span data-stu-id="f256b-151">**Restrict**</span></span>          | <span data-ttu-id="f256b-152">Brak</span><span class="sxs-lookup"><span data-stu-id="f256b-152">None</span></span>                                | <span data-ttu-id="f256b-153">Brak</span><span class="sxs-lookup"><span data-stu-id="f256b-153">None</span></span>                                  |

<span data-ttu-id="f256b-154">W tabelach powyżej *Brak* może doprowadzić do naruszenia ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="f256b-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="f256b-155">Na przykład jeśli jednostka jednostki/podrzędny jest usuwana, lecz nie podjęto żadnej akcji można zmienić klucza obcego z zależnych od ustawień lokalnych/podrzędny, następnie bazie danych prawdopodobnie zgłosi na SaveChanges z powodu naruszenia ograniczenia obcego.</span><span class="sxs-lookup"><span data-stu-id="f256b-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="f256b-156">Na wysokim poziomie:</span><span class="sxs-lookup"><span data-stu-id="f256b-156">At a high level:</span></span>
* <span data-ttu-id="f256b-157">Jeśli masz jednostek, które nie mogą istnieć bez klasy nadrzędnej, a EF automatyzującą automatycznego usuwania elementy podrzędne, a następnie użyć *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="f256b-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="f256b-158">Jednostki, które nie mogą znajdować się bez nadrzędnej zwykle należy na użytek wymaganych relacji, które *Cascade* jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="f256b-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="f256b-159">Jeśli masz jednostek, które mogą lub nie może mieć elementu nadrzędnego i EF, aby zadbać o nulling się klucz obcy dla Ciebie, a następnie użyć *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="f256b-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="f256b-160">Jednostek, które może istnieć bez nadrzędnej zwykle należy użyć opcjonalne relacji, dla którego *ClientSetNull* jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="f256b-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="f256b-161">Jeśli mają również próbować nawet propagację wartości null do podrzędnych klucze obce w bazie danych po jednostce podrzędnej nie został załadowany, następnie za pomocą *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="f256b-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="f256b-162">Jednak pamiętaj, że baza danych musi obsługiwać to Konfigurowanie bazy danych, np. to może spowodować inne ograniczenia, co w praktyce często sprawia, że ta opcja niepraktyczne.</span><span class="sxs-lookup"><span data-stu-id="f256b-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="f256b-163">Dlatego *SetNull* nie jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="f256b-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="f256b-164">Jeśli nie chcesz programu EF Core kiedykolwiek automatycznie Usuń jednostkę lub wartość null out klucza obcego, automatycznie, a następnie użyć *Ogranicz*.</span><span class="sxs-lookup"><span data-stu-id="f256b-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="f256b-165">Należy pamiętać, że wymaga to, że Twój kod Synchronizuj podrzędnych jednostek i ich wartości klucza obcego ręcznie w przeciwnym razie ograniczenie wyjątki zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="f256b-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="f256b-166">W programie EF Core, w odróżnieniu od EF6 efekty kaskadowych nie nawiązały natychmiast, ale zamiast tego tylko wtedy, gdy jest wywoływana SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f256b-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="f256b-167">**Zmiany w programie EF Core 2.0:** w poprzednich wersjach *Ogranicz* spowodowałoby opcjonalne właściwości klucza obcego w śledzonych jednostki zależne, należy ustawić wartości null i zostało domyślne zachowanie dla relacji opcjonalne przy usuwaniu.</span><span class="sxs-lookup"><span data-stu-id="f256b-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="f256b-168">W programie EF Core 2.0 *ClientSetNull* została wprowadzona do reprezentowania tego zachowania i stało się domyślnie w przypadku relacji opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="f256b-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="f256b-169">Zachowanie *Ogranicz* została dostosowana do nigdy nie ma żadnych efektów ubocznych na jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="f256b-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="f256b-170">Przykłady usuwania jednostki</span><span class="sxs-lookup"><span data-stu-id="f256b-170">Entity deletion examples</span></span>

<span data-ttu-id="f256b-171">Poniższy kod jest częścią [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="f256b-171">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="f256b-172">Przykład pokazuje, co się stanie dla każdego zachowanie dotyczące usuwania dla relacji wymagane i opcjonalne, po usunięciu obiektu nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="f256b-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="f256b-173">Przejdźmy teraz przez poszczególnych odmian, aby zrozumieć, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="f256b-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="f256b-174">DeleteBehavior.Cascade z relacją wymagane lub opcjonalne</span><span class="sxs-lookup"><span data-stu-id="f256b-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="f256b-175">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="f256b-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="f256b-176">Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="f256b-177">SaveChanges wysyła usuwa zarówno zależności/podrzędnych (wpisy), a następnie jednostkę/element nadrzędny (blog)</span><span class="sxs-lookup"><span data-stu-id="f256b-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="f256b-178">Po zapisaniu wszystkich jednostek są odłączone od teraz zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="f256b-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="f256b-179">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relację</span><span class="sxs-lookup"><span data-stu-id="f256b-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="f256b-180">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="f256b-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="f256b-181">Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="f256b-182">SaveChanges podejmuje próbę ustawienia wpisu klucza Obcego na wartość null, ale to nie powiodło się ponieważ klucza Obcego nie dopuszcza wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="f256b-183">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relację</span><span class="sxs-lookup"><span data-stu-id="f256b-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="f256b-184">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="f256b-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="f256b-185">Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="f256b-186">Próby SaveChanges ustawia klucza Obcego z obu zależności/elementów podrzędnych (ogłoszeń) o wartości null przed usunięciem jednostki/element nadrzędny (blog)</span><span class="sxs-lookup"><span data-stu-id="f256b-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="f256b-187">Po zapisaniu jednostki/element nadrzędny (blog) zostanie usunięty, ale zależności/elementy podrzędne (ogłoszeń) nadal są śledzone.</span><span class="sxs-lookup"><span data-stu-id="f256b-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="f256b-188">Śledzone zależności/elementy podrzędne (ogłoszeń) ma wartości null klucza Obcego, a ich odwołania do jednostki/nadrzędnych usunięte (blog) została usunięta.</span><span class="sxs-lookup"><span data-stu-id="f256b-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="f256b-189">DeleteBehavior.Restrict z relacją wymagane lub opcjonalne</span><span class="sxs-lookup"><span data-stu-id="f256b-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="f256b-190">Blog jest oznaczony jako usunięty</span><span class="sxs-lookup"><span data-stu-id="f256b-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="f256b-191">Wpisy początkowo pozostać Unchanged, ponieważ kaskady tak się nie stanie do momentu SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f256b-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="f256b-192">Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawić klucza Obcego o wartości null, pozostaje inną niż null, i SaveChanges zgłasza bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="f256b-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="f256b-193">Usuń porzucone przykłady</span><span class="sxs-lookup"><span data-stu-id="f256b-193">Delete orphans examples</span></span>

<span data-ttu-id="f256b-194">Poniższy kod jest częścią [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , można pobrać i uruchomić.</span><span class="sxs-lookup"><span data-stu-id="f256b-194">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="f256b-195">Przykład pokazuje, co się stanie dla każdego zachowanie dotyczące usuwania dla relacji wymagane i opcjonalne po relacji między nadrzędnym/jednostki i jego elementy podrzędne/dependents jest oddzielone.</span><span class="sxs-lookup"><span data-stu-id="f256b-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="f256b-196">W tym przykładzie relacji jest oddzielone przez usunięcie zależności/elementy podrzędne (ogłoszeń) z właściwości nawigacji kolekcji jednostki/nadrzędnej (blog).</span><span class="sxs-lookup"><span data-stu-id="f256b-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="f256b-197">Jednak to zachowanie jest taka sama Jeśli odwołanie z zależnych od ustawień lokalnych/podrzędny do podmiotu zabezpieczeń/element nadrzędny jest zamiast tego pustych.</span><span class="sxs-lookup"><span data-stu-id="f256b-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="f256b-198">Przejdźmy teraz przez poszczególnych odmian, aby zrozumieć, co się dzieje.</span><span class="sxs-lookup"><span data-stu-id="f256b-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="f256b-199">DeleteBehavior.Cascade z relacją wymagane lub opcjonalne</span><span class="sxs-lookup"><span data-stu-id="f256b-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="f256b-200">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="f256b-201">W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="f256b-202">SaveChanges wysyła usuwa dla zależności/dzieci (wpisów)</span><span class="sxs-lookup"><span data-stu-id="f256b-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="f256b-203">Po zapisaniu zależności/elementy podrzędne (ogłoszeń) są odłączone od teraz zostały usunięte z bazy danych</span><span class="sxs-lookup"><span data-stu-id="f256b-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="f256b-204">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z wymaganą relację</span><span class="sxs-lookup"><span data-stu-id="f256b-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="f256b-205">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="f256b-206">W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="f256b-207">SaveChanges podejmuje próbę ustawienia wpisu klucza Obcego na wartość null, ale to nie powiodło się ponieważ klucza Obcego nie dopuszcza wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="f256b-208">DeleteBehavior.ClientSetNull lub DeleteBehavior.SetNull z opcjonalną relację</span><span class="sxs-lookup"><span data-stu-id="f256b-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="f256b-209">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="f256b-210">W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="f256b-211">SaveChanges ustawia klucza Obcego z obu zależności/elementów podrzędnych (ogłoszeń) o wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="f256b-212">Po zapisaniu zależności/elementy podrzędne (ogłoszeń) ma wartości null klucza Obcego, a ich odwołania do jednostki/nadrzędnych usunięte (blog) została usunięta.</span><span class="sxs-lookup"><span data-stu-id="f256b-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="f256b-213">DeleteBehavior.Restrict z relacją wymagane lub opcjonalne</span><span class="sxs-lookup"><span data-stu-id="f256b-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="f256b-214">Wpisy są oznaczane jako zmodyfikowane, ponieważ severing relacji klucza Obcego być oznaczony jako wartość null</span><span class="sxs-lookup"><span data-stu-id="f256b-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="f256b-215">W przypadku klucza Obcego nie dopuszcza wartości null, następnie wartość rzeczywista nie zmieni, nawet jeśli nie jest oznaczona jako wartości null</span><span class="sxs-lookup"><span data-stu-id="f256b-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="f256b-216">Ponieważ *Ogranicz* informuje EF, aby automatycznie ustawić klucza Obcego o wartości null, pozostaje inną niż null, i SaveChanges zgłasza bez zapisywania</span><span class="sxs-lookup"><span data-stu-id="f256b-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="f256b-217">Kaskadowe nieśledzone jednostek</span><span class="sxs-lookup"><span data-stu-id="f256b-217">Cascading to untracked entities</span></span>

<span data-ttu-id="f256b-218">Gdy wywołujesz *SaveChanges*, reguły usuwanie kaskadowe zostaną zastosowane do dowolnej jednostki, które są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="f256b-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="f256b-219">Jest to sytuacja we wszystkich przykładach pokazano powyżej, co jest Dlaczego SQL został wygenerowany można usunąć jednostki/element nadrzędny (blog) i wszystkie zależności/elementy podrzędne (ogłoszeń):</span><span class="sxs-lookup"><span data-stu-id="f256b-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="f256b-220">Jeśli tylko podmiot zabezpieczeń jest ładowany — na przykład, gdy zapytania są tworzone dla bloga bez `Include(b => b.Posts)` też dołączyć wpisy — następnie SaveChanges będą generowane tylko SQL, aby usunąć jednostkę/element nadrzędny:</span><span class="sxs-lookup"><span data-stu-id="f256b-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="f256b-221">Zależności/elementy podrzędne (ogłoszeń) tylko zostaną usunięte, jeśli baza danych ma odpowiedni zachowania kaskadowe skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="f256b-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="f256b-222">Jeśli używasz programu EF utworzyć bazę danych, zachowania kaskadowe będzie Instalatora dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="f256b-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
