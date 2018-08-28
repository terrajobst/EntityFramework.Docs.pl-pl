---
title: Odłączone jednostki — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 51367d2619b1943c300f8954123f70b909ad96e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994402"
---
# <a name="disconnected-entities"></a><span data-ttu-id="c5bb3-102">Odłączone jednostki</span><span class="sxs-lookup"><span data-stu-id="c5bb3-102">Disconnected entities</span></span>

<span data-ttu-id="c5bb3-103">Wystąpienie typu DbContext będzie automatycznie śledzić obiektów zwracanych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="c5bb3-104">Zmiany wprowadzone do tych jednostek następnie zostanie wykryty, gdy SaveChanges nosi nazwę bazy danych zostaną zaktualizowane zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="c5bb3-105">Zobacz [podstawowe Zapisz](basic.md) i [powiązanych danych](related-data.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="c5bb3-106">Jednak czasami jednostki są wysyłane zapytanie przy użyciu jednego wystąpienia kontekstu i następnie zapisywane przy użyciu innego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="c5bb3-107">To często zdarza się w scenariuszach "odłączonego", takich jak aplikacja sieci web, gdzie jednostki są badane, wysłane do klienta, modyfikować, wysyłanych z powrotem do serwera w żądaniu i następnie zapisany.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="c5bb3-108">W tym przypadku kontekstu drugiego wystąpienia musi wiedzieć, czy jednostki są nowe (powinien zostać wstawiony) lub istniejące (powinien zostać zaktualizowany).</span><span class="sxs-lookup"><span data-stu-id="c5bb3-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="c5bb3-109">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="c5bb3-110">EF Core można śledzić tylko jedno wystąpienie jednostki z danej wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="c5bb3-111">Najlepszym sposobem, aby uniknąć tego problemu jest do użycia krótkotrwałe kontekstu dla każdej jednostki pracy w taki sposób, że kontekst zaczyna się puste, zawiera jednostki podłączone do niego, zapisuje te jednostki oraz kontekst jest usunięty i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="c5bb3-112">Identyfikowanie nowych jednostek</span><span class="sxs-lookup"><span data-stu-id="c5bb3-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="c5bb3-113">Klient identyfikuje nowe jednostki</span><span class="sxs-lookup"><span data-stu-id="c5bb3-113">Client identifies new entities</span></span>

<span data-ttu-id="c5bb3-114">Najprostszym przypadku radzenia sobie z jest, gdy klient informuje serwer, czy jednostka jest nowym lub istniejącym.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="c5bb3-115">Na przykład często żądania, aby wstawić nowy obiekt różni się od żądanie zaktualizowania istniejącej jednostki.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="c5bb3-116">Dalszej części tej sekcji omówiono przypadki gdzie go, które są niezbędne określić w inny sposób, czy należy wstawić lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="c5bb3-117">Za pomocą automatycznego generowania kluczy</span><span class="sxs-lookup"><span data-stu-id="c5bb3-117">With auto-generated keys</span></span>

<span data-ttu-id="c5bb3-118">Wartość automatycznie generowanego klucza często może służyć do określenia, czy jednostka musi być wstawiane lub aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="c5bb3-119">Jeśli nie ustawiono klucza (oznacza to, że nadal ma wartość domyślną CLR o wartości null, wartość zero, itp.), a jednostka musi być nowe musi wstawiania.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="c5bb3-120">Z drugiej strony Jeśli ustawiono wartość klucza, następnie go musi już zostały wcześniej zapisane i teraz wymaga aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="c5bb3-121">Innymi słowy Jeśli klucz ma wartość, a następnie jednostki badano wysłane do klienta i ma teraz wróć do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="c5bb3-122">Jest łatwy do sprawdzenia nie ustawiono klucza, gdy znana jest typem jednostki:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="c5bb3-123">EF ma również wbudowane sposobem wykonania tego typu jednostki i typ klucza:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="c5bb3-124">Klucze są ustawione, tak szybko, jak jednostki są śledzone od kontekstu, nawet jeśli jednostka jest w stanie dodany.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="c5bb3-125">Dzięki temu podczas przechodzenia między wykres jednostek i podejmowania decyzji o postępowaniu z każdego, na przykład, jak za pomocą interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="c5bb3-126">Wartość klucza powinna służyć wyłącznie w taki sposób, w tym miejscu pokazano _przed_ wszelkie rozmowy do śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="c5bb3-127">Przy użyciu innych kluczy</span><span class="sxs-lookup"><span data-stu-id="c5bb3-127">With other keys</span></span>

<span data-ttu-id="c5bb3-128">Niektóre inny mechanizm jest potrzebny do identyfikowania nowych jednostek, gdy wartości klucza nie są generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="c5bb3-129">Istnieją dwa ogólne podejścia do tego:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="c5bb3-130">Zapytanie dla jednostki</span><span class="sxs-lookup"><span data-stu-id="c5bb3-130">Query for the entity</span></span>
 * <span data-ttu-id="c5bb3-131">Przekazać flagę od klienta</span><span class="sxs-lookup"><span data-stu-id="c5bb3-131">Pass a flag from the client</span></span>

<span data-ttu-id="c5bb3-132">Aby wysłać zapytanie do jednostki, po prostu użyj metody Find:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="c5bb3-133">Jest poza zakres tego dokumentu, aby wyświetlić pełny kod do przekazywania flagę od klienta.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="c5bb3-134">W aplikacji sieci web zwykle oznacza to, co innych żądań dla rozmaitych akcji lub przekazywanie pewnego stanu w żądaniu, a następnie wyodrębniania go w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="c5bb3-135">Zapisywanie pojedynczych jednostek</span><span class="sxs-lookup"><span data-stu-id="c5bb3-135">Saving single entities</span></span>

<span data-ttu-id="c5bb3-136">Jeśli wiadomo, czy jest potrzebny insert nebo update, a następnie dodaj lub zaktualizuj można odpowiednio:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="c5bb3-137">Jednak jeśli jednostki używa automatycznego generowania wartości klucza, następnie metoda aktualizacji może służyć w obu przypadkach:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="c5bb3-138">Metoda aktualizacji zazwyczaj oznacza jednostkę do aktualizacji, wstawiania nie.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="c5bb3-139">Jednakże jeśli jednostka ma klucz wygenerowany automatycznie, a nie wartość klucza została ustawiona, a następnie jednostki zamiast tego jest automatycznie oznaczony do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="c5bb3-140">To zachowanie została wprowadzona w programie EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="c5bb3-141">Dla wcześniejszych wersji należy zawsze jawnie wybrać Dodawanie lub aktualizowanie.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="c5bb3-142">Jeśli jednostka nie korzysta z automatycznego generowania kluczy, a następnie aplikacja musi zdecydować, czy wstawione lub zaktualizowane jednostki: na przykład:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="c5bb3-143">Dostępne są następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-143">The steps here are:</span></span>
* <span data-ttu-id="c5bb3-144">Jeśli dodasz Znajdź zwraca wartość null, a następnie baza danych nie zawiera jeszcze blog o tym identyfikatorze, dlatego nazywamy Oznacz ją do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="c5bb3-145">Jeśli wyszukiwanie zwraca jednostkę, następnie istnieje w bazie danych i kontekstu jest teraz śledzenie istniejącej jednostki</span><span class="sxs-lookup"><span data-stu-id="c5bb3-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="c5bb3-146">Następnie używamy SetValues można ustawić wartości dla wszystkich właściwości dla tej jednostki do tych, które pochodzą od klienta.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="c5bb3-147">Wywołanie SetValues spowoduje oznaczenie jednostkę którą chcesz zaktualizować, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="c5bb3-148">SetValues oznaczy tylko zmienione właściwości, które mają różne wartości do tych w jednostce śledzone.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="c5bb3-149">Oznacza to, że gdy aktualizacja jest wysyłana, tylko te kolumny, które rzeczywiście zostały zmienione zostaną zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="c5bb3-150">(I jeśli nic się nie zmieniło, aktualizacja nie zostanie wysłana w ogóle)</span><span class="sxs-lookup"><span data-stu-id="c5bb3-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="c5bb3-151">Praca z wykresami</span><span class="sxs-lookup"><span data-stu-id="c5bb3-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="c5bb3-152">Rozwiązanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="c5bb3-152">Identity resolution</span></span>

<span data-ttu-id="c5bb3-153">Jak wspomniano powyżej, programem EF Core tylko śledzić jednego wystąpienia z danej wartości klucza podstawowego jednostki.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="c5bb3-154">Pracując z wykresami wykres najlepiej powinny być tworzone, tak, aby ta niezmiennej jest utrzymywany i kontekst powinien być używany dla tylko jednej jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="c5bb3-155">Jeśli wykres zawiera duplikaty, następnie będzie konieczne do przetworzenia na wykresie przed wysłaniem ich do programu EF w celu skonsolidowania wielu wystąpień w jednym.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="c5bb3-156">To może nie być prosta których wystąpienia mają wartościami będącymi w konflikcie i relacje, dlatego konsolidację duplikaty powinno się odbywać szybko, jak to możliwe w potoku aplikacji, aby uniknąć konfliktów.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="c5bb3-157">Wszystkie nowe/wszystkich istniejących jednostek.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-157">All new/all existing entities</span></span>

<span data-ttu-id="c5bb3-158">Przykładem korzystania z wykresów jest wstawianie lub aktualizowania blogu wraz z jego kolekcja skojarzone wpisów.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="c5bb3-159">Powinien zostać wstawiony wszystkich jednostek w wykresie lub wszystkie powinny być aktualizowane, następnie proces jest taka sama, jak opisano powyżej dla jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="c5bb3-160">Na przykład wykres blogów i wpisów umieszczonych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="c5bb3-161">mogą być wstawiane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="c5bb3-162">Spowoduje to oznaczenie blogu i wszystkie wpisy, które ma zostać wstawiony wywołania do Add.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="c5bb3-163">Podobnie jeśli wszystkie jednostki w grafie muszą zostać zaktualizowane, wówczas aktualizacji mogą być używane:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="c5bb3-164">Blog i wszystkie jego wpisy zostaną oznaczone do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="c5bb3-165">Kombinacja nowych i istniejących jednostek</span><span class="sxs-lookup"><span data-stu-id="c5bb3-165">Mix of new and existing entities</span></span>

<span data-ttu-id="c5bb3-166">Za pomocą automatycznego generowania kluczy aktualizacja ponownie można zarówno dla operacji wstawienia i aktualizacje, nawet jeśli wykres zawiera różne jednostki, które wymagają, wstawianie i tych, które wymagają zaktualizowania:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="c5bb3-167">Aktualizacja spowoduje oznaczenie dowolnej jednostki w wykresu, blogu lub wpis do wstawienia nie zainstalowano zestawu wartości klucza, podczas gdy inne jednostki są oznaczone do aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="c5bb3-168">Jak wcześniej, bez korzystania z automatycznego generowania kluczy, zapytanie i jakieś operacje przetwarzania może być użyty:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="c5bb3-169">Obsługa usuwa</span><span class="sxs-lookup"><span data-stu-id="c5bb3-169">Handling deletes</span></span>

<span data-ttu-id="c5bb3-170">Delete mogą być trudne do obsługi, ponieważ często Brak jednostki oznacza, że go usunąć.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="c5bb3-171">Jednym ze sposobów, aby poradzić sobie z tym jest używać "usuwania nietrwałego" w taki sposób, że jednostka jest oznaczony jako usunięty zamiast rzeczywistości usuwane.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="c5bb3-172">Usuwa, a następnie staje się taka sama jak aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="c5bb3-173">Usuwanie nietrwałego może być implementowany w przy użyciu [zapytania filtry](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="c5bb3-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="c5bb3-174">Usuwa wartość true, aby uzyskać wspólny wzorzec jest użycie rozszerzenia wzorca zapytania do wykonania, co to jest zasadniczo różnica wykresu Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="c5bb3-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="c5bb3-175">TrackGraph</span></span>

<span data-ttu-id="c5bb3-176">Wewnętrznie, Dodaj, Dołącz i aktualizacji za pomocą przechodzenie grafu przy podejmowaniu wprowadzone dla każdej jednostki do tego, czy jego powinien być oznaczony jako dodano (Aby wstawić), zmodyfikowany (w celu aktualizacji), Unchanged (NIC), lub usunięte (Aby usunąć).</span><span class="sxs-lookup"><span data-stu-id="c5bb3-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="c5bb3-177">Ten mechanizm jest uwidaczniany za pomocą interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="c5bb3-178">Na przykład załóżmy, że gdy klient wysyła z powrotem wykres jednostek ustawia niektóre flagi dla każdej jednostki wskazujący sposób obsługi.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="c5bb3-179">TrackGraph następnie może służyć do przetwarzania tej flagi:</span><span class="sxs-lookup"><span data-stu-id="c5bb3-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="c5bb3-180">Flagi są wyświetlane tylko w ramach jednostki dla uproszczenia w przykładzie.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="c5bb3-181">Zazwyczaj flagi powinien być częścią obiekt DTO lub inny stan, zawarty w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="c5bb3-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
