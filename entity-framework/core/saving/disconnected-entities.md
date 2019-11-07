---
title: Rozłączone jednostki — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 88c3fa8ea5b8246a932f5cf21e674bc7cc71c0ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656269"
---
# <a name="disconnected-entities"></a><span data-ttu-id="d3538-102">Rozłączone jednostki</span><span class="sxs-lookup"><span data-stu-id="d3538-102">Disconnected entities</span></span>

<span data-ttu-id="d3538-103">Wystąpienie DbContext automatycznie śledzi jednostki zwrócone z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3538-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="d3538-104">Zmiany wprowadzone w tych jednostkach zostaną następnie wykryte, gdy zostanie wywołana metody SaveChanges i baza danych zostanie zaktualizowana w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="d3538-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="d3538-105">Szczegóły można znaleźć w temacie Podstawowe dane dotyczące [zapisywania](basic.md) i [pokrewnych danych](related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="d3538-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="d3538-106">Jednak czasami do jednostek są wysyłane zapytania przy użyciu jednego wystąpienia kontekstu, a następnie zapisane przy użyciu innego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="d3538-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="d3538-107">Często zdarza się to w scenariuszach "rozłączonych", takich jak aplikacja sieci Web, do której są wysyłane zapytania, wysyłany do klienta, modyfikowane, wysyłane z powrotem do serwera w żądaniu, a następnie zapisane.</span><span class="sxs-lookup"><span data-stu-id="d3538-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="d3538-108">W takim przypadku drugie wystąpienie kontekstu musi wiedzieć, czy jednostki są nowe (powinny być wstawiane) czy istniejące (należy je zaktualizować).</span><span class="sxs-lookup"><span data-stu-id="d3538-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

<!-- markdownlint-disable MD028 -->
> [!TIP]
> <span data-ttu-id="d3538-109">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="d3538-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="d3538-110">EF Core może śledzić tylko jedno wystąpienie dowolnej jednostki z daną wartością klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="d3538-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="d3538-111">Najlepszym sposobem na uniknięcie tego problemu jest użycie kontekstu krótkotrwałego dla każdej jednostki pracy w taki sposób, że kontekst zaczyna puste, ma dołączone jednostki, zapisuje te jednostki, a następnie kontekst zostaje usunięty i odrzucony.</span><span class="sxs-lookup"><span data-stu-id="d3538-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a><span data-ttu-id="d3538-112">Identyfikowanie nowych jednostek</span><span class="sxs-lookup"><span data-stu-id="d3538-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="d3538-113">Klient identyfikuje nowe jednostki</span><span class="sxs-lookup"><span data-stu-id="d3538-113">Client identifies new entities</span></span>

<span data-ttu-id="d3538-114">Najprostszym przypadkiem, w którym należy się zająć, jest to, kiedy klient informuje serwer o tym, czy jednostka jest nowa czy istniejąca.</span><span class="sxs-lookup"><span data-stu-id="d3538-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="d3538-115">Na przykład często żądanie wstawienia nowej jednostki różni się od żądania zaktualizowania istniejącej jednostki.</span><span class="sxs-lookup"><span data-stu-id="d3538-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="d3538-116">Pozostała część tej sekcji obejmuje przypadki, w których konieczne jest określenie w inny sposób, czy należy wstawić czy zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="d3538-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="d3538-117">Z kluczami generowanymi automatycznie</span><span class="sxs-lookup"><span data-stu-id="d3538-117">With auto-generated keys</span></span>

<span data-ttu-id="d3538-118">Wartość automatycznie generowanego klucza może być często używana do określenia, czy należy wstawić lub zaktualizować jednostkę.</span><span class="sxs-lookup"><span data-stu-id="d3538-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="d3538-119">Jeśli klucz nie został ustawiony (oznacza to, że nadal ma wartość domyślną CLR o wartości null, zero itp.), jednostka musi być nowa i musi wstawiać.</span><span class="sxs-lookup"><span data-stu-id="d3538-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="d3538-120">Z drugiej strony, jeśli wartość klucza została ustawiona, należy ją wcześniej zapisać i teraz wymaga aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="d3538-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="d3538-121">Innymi słowy, jeśli klucz ma wartość, jednostka została zbadana, wysłana do klienta i teraz ponownie zostanie zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="d3538-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="d3538-122">Jeśli wiadomo, że typ jednostki jest znany:</span><span class="sxs-lookup"><span data-stu-id="d3538-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="d3538-123">Jednakże EF ma wbudowaną metodę, aby to zrobić dla dowolnego typu jednostki i typu klucza:</span><span class="sxs-lookup"><span data-stu-id="d3538-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="d3538-124">Klucze są ustawiane, gdy tylko jednostki są śledzone przez kontekst, nawet jeśli jednostka jest w stanie dodany.</span><span class="sxs-lookup"><span data-stu-id="d3538-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="d3538-125">Ułatwia to przechodzenie grafu jednostek i decydowanie o tym, co należy zrobić z każdym z nich, na przykład w przypadku korzystania z interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="d3538-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="d3538-126">Wartość klucza powinna być używana tylko w sposób przedstawiony tutaj _przed_ wywołaniem do śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="d3538-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="d3538-127">Z innymi kluczami</span><span class="sxs-lookup"><span data-stu-id="d3538-127">With other keys</span></span>

<span data-ttu-id="d3538-128">Aby identyfikować nowe jednostki w przypadku, gdy wartości kluczy nie są generowane automatycznie, jest wymagany inny mechanizm.</span><span class="sxs-lookup"><span data-stu-id="d3538-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="d3538-129">Istnieją dwa ogólne podejścia do tego:</span><span class="sxs-lookup"><span data-stu-id="d3538-129">There are two general approaches to this:</span></span>

* <span data-ttu-id="d3538-130">Zapytanie dotyczące jednostki</span><span class="sxs-lookup"><span data-stu-id="d3538-130">Query for the entity</span></span>
* <span data-ttu-id="d3538-131">Przekaż flagę z klienta</span><span class="sxs-lookup"><span data-stu-id="d3538-131">Pass a flag from the client</span></span>

<span data-ttu-id="d3538-132">Aby wykonać zapytanie dotyczące jednostki, po prostu Użyj metody Find:</span><span class="sxs-lookup"><span data-stu-id="d3538-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="d3538-133">Ten dokument wykracza poza zakres tego dokumentu, aby pokazać pełen kod przekazywania flagi z klienta.</span><span class="sxs-lookup"><span data-stu-id="d3538-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="d3538-134">W aplikacji sieci Web zazwyczaj oznacza to, że różne żądania dla różnych akcji lub przekazywanie pewnych stanów w żądaniu są wyodrębniane w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="d3538-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="d3538-135">Zapisywanie pojedynczych jednostek</span><span class="sxs-lookup"><span data-stu-id="d3538-135">Saving single entities</span></span>

<span data-ttu-id="d3538-136">Jeśli wiadomo, czy jest wymagana INSERT lub Update, można odpowiednio użyć opcji Dodaj lub zaktualizuj:</span><span class="sxs-lookup"><span data-stu-id="d3538-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="d3538-137">Jeśli jednak jednostka używa automatycznie generowanych wartości kluczy, metoda Update może być używana w obu przypadkach:</span><span class="sxs-lookup"><span data-stu-id="d3538-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="d3538-138">Metoda Update zwykle oznacza jednostkę do aktualizacji, nie wstawiaj.</span><span class="sxs-lookup"><span data-stu-id="d3538-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="d3538-139">Jeśli jednak jednostka ma automatycznie wygenerowany klucz i żadna wartość klucza nie została ustawiona, jednostka zostanie automatycznie oznaczona do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="d3538-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="d3538-140">To zachowanie zostało wprowadzone w EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="d3538-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="d3538-141">W przypadku wcześniejszych wersji zawsze konieczne jest jawne wybranie opcji Dodaj lub zaktualizuj.</span><span class="sxs-lookup"><span data-stu-id="d3538-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="d3538-142">Jeśli jednostka nie używa automatycznie generowanych kluczy, aplikacja musi zdecydować, czy jednostka powinna zostać wstawiona lub zaktualizowana: na przykład:</span><span class="sxs-lookup"><span data-stu-id="d3538-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="d3538-143">Poniżej przedstawiono następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="d3538-143">The steps here are:</span></span>

* <span data-ttu-id="d3538-144">Jeśli funkcja Znajdź zwraca wartość null, baza danych nie zawiera jeszcze blogu o tym IDENTYFIKATORze, więc wywołamy polecenie Dodaj markę do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="d3538-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="d3538-145">Jeśli funkcja Znajdź zwraca jednostkę, w bazie danych istnieje wartość, a w kontekście jest teraz śledzona Istniejąca jednostka</span><span class="sxs-lookup"><span data-stu-id="d3538-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="d3538-146">Następnie użyjemy setValues, aby ustawić wartości dla wszystkich właściwości tej jednostki dla tych, które zostały dostarczone przez klienta.</span><span class="sxs-lookup"><span data-stu-id="d3538-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="d3538-147">Wywołanie setValues oznaczy jednostkę, która ma zostać zaktualizowana w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="d3538-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="d3538-148">Wartości setValues będą oznaczane jako zmodyfikowane właściwości, które mają różne wartości w monitorowanej jednostce.</span><span class="sxs-lookup"><span data-stu-id="d3538-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="d3538-149">Oznacza to, że po wysłaniu aktualizacji zostaną zaktualizowane tylko te kolumny, które zostały zmienione w rzeczywistości.</span><span class="sxs-lookup"><span data-stu-id="d3538-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="d3538-150">(A jeśli nic się nie zmieniło, aktualizacja nie zostanie wysłana wcale.)</span><span class="sxs-lookup"><span data-stu-id="d3538-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="d3538-151">Praca z wykresami</span><span class="sxs-lookup"><span data-stu-id="d3538-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="d3538-152">Rozpoznawanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="d3538-152">Identity resolution</span></span>

<span data-ttu-id="d3538-153">Jak wspomniano powyżej, EF Core może śledzić tylko jedno wystąpienie dowolnej jednostki z daną wartością klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="d3538-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="d3538-154">Podczas pracy z wykresami, wykres powinien być utworzony w taki sposób, że niezmienna jest utrzymywana, a kontekst powinien być używany tylko dla jednej jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="d3538-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="d3538-155">Jeśli wykres zawiera duplikaty, konieczne będzie przetworzenie wykresu przed wysłaniem go do EF, aby skonsolidować wiele wystąpień w jeden.</span><span class="sxs-lookup"><span data-stu-id="d3538-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="d3538-156">Może to nie być nieuproszczone miejsce, w którym wystąpienia mają wartości powodujące konflikt i relacje, dlatego konsolidacja duplikatów powinna być wykonywana najszybciej, jak to możliwe w potoku aplikacji, aby uniknąć rozwiązywania konfliktów.</span><span class="sxs-lookup"><span data-stu-id="d3538-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="d3538-157">Wszystkie nowe/wszystkie istniejące jednostki</span><span class="sxs-lookup"><span data-stu-id="d3538-157">All new/all existing entities</span></span>

<span data-ttu-id="d3538-158">Przykładem pracy z wykresami jest wstawianie lub aktualizowanie blogu wraz z kolekcją skojarzonych wpisów.</span><span class="sxs-lookup"><span data-stu-id="d3538-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="d3538-159">Jeśli wszystkie jednostki na grafie powinny zostać wstawione lub wszystkie powinny zostać zaktualizowane, proces jest taki sam jak w powyższym obszarze dla pojedynczych jednostek.</span><span class="sxs-lookup"><span data-stu-id="d3538-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="d3538-160">Na przykład Graf blogów i wpisów utworzonych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d3538-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="d3538-161">można go wstawić w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="d3538-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="d3538-162">Wywołanie do dodania spowoduje oznaczenie blogu i wszystkich wpisów do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="d3538-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="d3538-163">Podobnie, jeśli wszystkie jednostki na grafie muszą zostać zaktualizowane, można użyć aktualizacji:</span><span class="sxs-lookup"><span data-stu-id="d3538-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="d3538-164">Blog i wszystkie jego wpisy zostaną oznaczone jako do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="d3538-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="d3538-165">Mieszanie nowych i istniejących jednostek</span><span class="sxs-lookup"><span data-stu-id="d3538-165">Mix of new and existing entities</span></span>

<span data-ttu-id="d3538-166">Dzięki automatycznie generowanym kluczom aktualizacja może być ponownie używana dla operacji INSERT i Update, nawet jeśli wykres zawiera różne jednostki, które wymagają wstawiania i te, które wymagają aktualizacji:</span><span class="sxs-lookup"><span data-stu-id="d3538-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="d3538-167">Aktualizacja spowoduje oznaczenie dowolnej jednostki w grafie, blogu lub wpisie do wstawienia, jeśli nie ma ustawionej wartości klucza, a wszystkie inne jednostki są oznaczone do aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="d3538-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="d3538-168">Tak jak wcześniej, gdy nie korzystasz z kluczy generowanych automatycznie, można użyć zapytania i niektórych operacji przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="d3538-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="d3538-169">Obsługa usunięć</span><span class="sxs-lookup"><span data-stu-id="d3538-169">Handling deletes</span></span>

<span data-ttu-id="d3538-170">Usuwanie może być trudne do obsłużenia, ponieważ często nie istnieje nieobecność jednostki, ponieważ należy ją usunąć.</span><span class="sxs-lookup"><span data-stu-id="d3538-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="d3538-171">Jednym ze sposobów postępowania z tym jest użycie "Usuwanie miękkie" w taki sposób, że jednostka jest oznaczona jako usunięta, a nie jest faktycznie usuwana.</span><span class="sxs-lookup"><span data-stu-id="d3538-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="d3538-172">Następnie usuwane są takie same jak aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="d3538-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="d3538-173">Usuwanie miękkie można zaimplementować przy użyciu [filtrów zapytań](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="d3538-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="d3538-174">W przypadku usunięć "true" typowym wzorcem jest użycie rozszerzenia wzorca zapytania, aby wykonać to, co jest zasadniczo różnicą wykresu.</span><span class="sxs-lookup"><span data-stu-id="d3538-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff.</span></span> <span data-ttu-id="d3538-175">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d3538-175">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="d3538-176">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="d3538-176">TrackGraph</span></span>

<span data-ttu-id="d3538-177">Wewnętrznie, dodawać, dołączać i aktualizować użycie programu Graph-przechodzenie przy użyciu oznaczeń wykonanych dla każdej jednostki w taki sposób, że powinna zostać oznaczona jako dodana (do wstawienia), zmodyfikowana (do aktualizacji), niezmieniona (nic nie dotyczy) lub usunięta (do usunięcia).</span><span class="sxs-lookup"><span data-stu-id="d3538-177">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="d3538-178">Ten mechanizm jest udostępniany za pośrednictwem interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="d3538-178">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="d3538-179">Załóżmy na przykład, że gdy klient wysyła wykres z tyłu jednostek, ustawia dla każdej jednostki flagę wskazującą, jak powinna być obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="d3538-179">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="d3538-180">TrackGraph można następnie użyć do przetworzenia tej flagi:</span><span class="sxs-lookup"><span data-stu-id="d3538-180">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="d3538-181">Flagi są wyświetlane tylko jako część jednostki dla uproszczenia przykładu.</span><span class="sxs-lookup"><span data-stu-id="d3538-181">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="d3538-182">Zwykle flagi będą częścią DTO lub innego stanu zawartego w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="d3538-182">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
