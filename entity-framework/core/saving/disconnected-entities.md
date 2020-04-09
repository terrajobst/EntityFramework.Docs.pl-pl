---
title: Odłączone jednostki — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417598"
---
# <a name="disconnected-entities"></a><span data-ttu-id="60e39-102">Odłączone encje</span><span class="sxs-lookup"><span data-stu-id="60e39-102">Disconnected entities</span></span>

<span data-ttu-id="60e39-103">Wystąpienie DbContext będzie automatycznie śledzić jednostki zwrócone z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="60e39-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="60e39-104">Zmiany wprowadzone w tych jednostkach zostaną wykryte po wywołaniu savechanges i aktualizacji bazy danych w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="60e39-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="60e39-105">Szczegółowe informacje można znaleźć w [podstawowych danych zapisu](basic.md) i [powiązanych danych.](related-data.md)</span><span class="sxs-lookup"><span data-stu-id="60e39-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="60e39-106">Jednak czasami jednostki są wyszukiwane przy użyciu jednego wystąpienia kontekstu, a następnie zapisywane przy użyciu innego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="60e39-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="60e39-107">Dzieje się tak często w scenariuszach "rozłączonych", takich jak aplikacja sieci web, w której jednostki są poszukiwane, wysyłane do klienta, modyfikowane, wysyłane z powrotem do serwera w żądaniu, a następnie zapisywane.</span><span class="sxs-lookup"><span data-stu-id="60e39-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="60e39-108">W takim przypadku drugie wystąpienie kontekstu musi wiedzieć, czy jednostki są nowe (powinny być wstawiane) lub istniejące (powinny być aktualizowane).</span><span class="sxs-lookup"><span data-stu-id="60e39-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

<!-- markdownlint-disable MD028 -->
> [!TIP]
> <span data-ttu-id="60e39-109">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="60e39-109">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="60e39-110">EF Core może śledzić tylko jedno wystąpienie dowolnej jednostki o danej wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="60e39-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="60e39-111">Najlepszym sposobem uniknięcia tego problemu jest użycie kontekstu krótkotrwałego dla każdej jednostki pracy, tak aby kontekst zaczyna się pusty, ma jednostki dołączone do niego, zapisuje te jednostki, a następnie kontekst jest usuwany i odrzucany.</span><span class="sxs-lookup"><span data-stu-id="60e39-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a><span data-ttu-id="60e39-112">Identyfikowanie nowych jednostek</span><span class="sxs-lookup"><span data-stu-id="60e39-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="60e39-113">Klient identyfikuje nowe jednostki</span><span class="sxs-lookup"><span data-stu-id="60e39-113">Client identifies new entities</span></span>

<span data-ttu-id="60e39-114">Najprostszym przypadkiem do czynienia jest, gdy klient informuje serwer, czy jednostka jest nowy lub istniejących.</span><span class="sxs-lookup"><span data-stu-id="60e39-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="60e39-115">Na przykład często żądanie wstawienia nowej jednostki różni się od żądania aktualizacji istniejącej jednostki.</span><span class="sxs-lookup"><span data-stu-id="60e39-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="60e39-116">Pozostała część tej sekcji obejmuje przypadki, w których konieczne jest określenie w inny sposób, czy wstawić lub zaktualizować.</span><span class="sxs-lookup"><span data-stu-id="60e39-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="60e39-117">Z automatycznie generowanymi klawiszami</span><span class="sxs-lookup"><span data-stu-id="60e39-117">With auto-generated keys</span></span>

<span data-ttu-id="60e39-118">Wartość automatycznie wygenerowanego klucza może być często używana do określenia, czy jednostka musi zostać wstawiona lub zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="60e39-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="60e39-119">Jeśli klucz nie został ustawiony (oznacza to, że nadal ma domyślną wartość CLR null, zero itp.), jednostka musi być nowa i wymaga wstawienia.</span><span class="sxs-lookup"><span data-stu-id="60e39-119">If the key has not been set (that is, it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="60e39-120">Z drugiej strony, jeśli wartość klucza została ustawiona, musi ona być już wcześniej zapisana, a teraz wymaga aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="60e39-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="60e39-121">Innymi słowy, jeśli klucz ma wartość, a następnie jednostka została zapytana, wysłane do klienta, a teraz wrócić do aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="60e39-121">In other words, if the key has a value, then the entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="60e39-122">Łatwo jest sprawdzić, czy nie ustawiono klucza, gdy typ jednostki jest znany:</span><span class="sxs-lookup"><span data-stu-id="60e39-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="60e39-123">Jednak EF ma również wbudowany sposób, aby to zrobić dla dowolnego typu jednostki i typu klucza:</span><span class="sxs-lookup"><span data-stu-id="60e39-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="60e39-124">Klucze są ustawiane tak szybko, jak jednostki są śledzone przez kontekst, nawet jeśli jednostka jest w stanie Dodane.</span><span class="sxs-lookup"><span data-stu-id="60e39-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="60e39-125">Pomaga to podczas przechodzenia przez wykres jednostek i podejmowania decyzji, co zrobić z każdym, takich jak podczas korzystania z interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="60e39-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="60e39-126">Wartość klucza powinna być używana tylko w sposób pokazany w tym miejscu _przed_ każdym wywołaniem do śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="60e39-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="60e39-127">Z innymi klawiszami</span><span class="sxs-lookup"><span data-stu-id="60e39-127">With other keys</span></span>

<span data-ttu-id="60e39-128">Jakiś inny mechanizm jest potrzebny do identyfikowania nowych jednostek, gdy wartości klucza nie są generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="60e39-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="60e39-129">Istnieją dwa ogólne podejścia do tego:</span><span class="sxs-lookup"><span data-stu-id="60e39-129">There are two general approaches to this:</span></span>

* <span data-ttu-id="60e39-130">Kwerenda dla encji</span><span class="sxs-lookup"><span data-stu-id="60e39-130">Query for the entity</span></span>
* <span data-ttu-id="60e39-131">Przekazywanie flagi od klienta</span><span class="sxs-lookup"><span data-stu-id="60e39-131">Pass a flag from the client</span></span>

<span data-ttu-id="60e39-132">Aby zbadać encję, wystarczy użyć metody Znajdź:</span><span class="sxs-lookup"><span data-stu-id="60e39-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="60e39-133">Jest poza zakresem tego dokumentu, aby wyświetlić pełny kod do przekazywania flagi od klienta.</span><span class="sxs-lookup"><span data-stu-id="60e39-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="60e39-134">W aplikacji sieci web zwykle oznacza dokonywanie różnych żądań dla różnych akcji lub przekazywanie niektórych stanów w żądaniu, a następnie wyodrębnianie go w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="60e39-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="60e39-135">Zapisywanie pojedynczych encji</span><span class="sxs-lookup"><span data-stu-id="60e39-135">Saving single entities</span></span>

<span data-ttu-id="60e39-136">Jeśli wiadomo, czy potrzebne jest wstawianie lub aktualizowanie, można odpowiednio użyć funkcji Dodaj lub Aktualizuj:</span><span class="sxs-lookup"><span data-stu-id="60e39-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="60e39-137">Jeśli jednak jednostka używa automatycznie generowanych wartości klucza, w obu przypadkach można użyć metody Update:</span><span class="sxs-lookup"><span data-stu-id="60e39-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="60e39-138">Update Metoda zwykle oznacza jednostki do aktualizacji, a nie wstawiania.</span><span class="sxs-lookup"><span data-stu-id="60e39-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="60e39-139">Jeśli jednak encja ma klucz generowany automatycznie i nie ustawiono żadnej wartości klucza, jednostka jest automatycznie oznaczana do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="60e39-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="60e39-140">To zachowanie zostało wprowadzone w EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="60e39-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="60e39-141">W przypadku wcześniejszych wydań zawsze należy jawnie wybrać dodaj lub aktualizuj.</span><span class="sxs-lookup"><span data-stu-id="60e39-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="60e39-142">Jeśli encja nie używa automatycznie generowanych kluczy, aplikacja musi zdecydować, czy encja powinna zostać wstawiona, czy aktualizowana: Na przykład:</span><span class="sxs-lookup"><span data-stu-id="60e39-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="60e39-143">Kroki są następujące:</span><span class="sxs-lookup"><span data-stu-id="60e39-143">The steps here are:</span></span>

* <span data-ttu-id="60e39-144">Jeśli funkcja Znajdź zwraca wartość null, baza danych nie zawiera jeszcze bloga o tym identyfikatorze, więc nazywamy Dodaj oznacz go do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="60e39-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="60e39-145">Jeśli funkcja Znajdź zwraca encję, istnieje ona w bazie danych, a kontekst jest teraz śledzeniu istniejącej encji</span><span class="sxs-lookup"><span data-stu-id="60e39-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="60e39-146">Następnie używamy SetValues, aby ustawić wartości dla wszystkich właściwości tej jednostki na te, które pochodziły od klienta.</span><span class="sxs-lookup"><span data-stu-id="60e39-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="60e39-147">Wywołanie SetValues oznaczy encję, która ma zostać zaktualizowana w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="60e39-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="60e39-148">SetValues oznaczy tylko jako zmodyfikowane właściwości, które mają inne wartości do tych w śledzonej jednostki.</span><span class="sxs-lookup"><span data-stu-id="60e39-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="60e39-149">Oznacza to, że po wysłaniu aktualizacji zostaną zaktualizowane tylko te kolumny, które faktycznie uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="60e39-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="60e39-150">(A jeśli nic się nie zmieniło, żadna aktualizacja nie zostanie wysłana w ogóle.)</span><span class="sxs-lookup"><span data-stu-id="60e39-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="60e39-151">Praca z wykresami</span><span class="sxs-lookup"><span data-stu-id="60e39-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="60e39-152">Rozpoznawanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="60e39-152">Identity resolution</span></span>

<span data-ttu-id="60e39-153">Jak wspomniano powyżej, EF Core można śledzić tylko jedno wystąpienie dowolnej jednostki z danej wartości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="60e39-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="60e39-154">Podczas pracy z wykresami wykres powinien być idealnie utworzony w taki sposób, aby ten niezmienny był zachowywany, a kontekst powinien być używany tylko dla jednej jednostki pracy.</span><span class="sxs-lookup"><span data-stu-id="60e39-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="60e39-155">Jeśli wykres zawiera duplikaty, konieczne będzie przetworzenie wykresu przed wysłaniem go do EF, aby skonsolidować wiele wystąpień w jeden.</span><span class="sxs-lookup"><span data-stu-id="60e39-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="60e39-156">Może to nie być trywialne, gdy wystąpienia mają sprzeczne wartości i relacje, więc konsolidacji duplikatów należy wykonać tak szybko, jak to możliwe w potoku aplikacji, aby uniknąć rozwiązywania konfliktów.</span><span class="sxs-lookup"><span data-stu-id="60e39-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="60e39-157">Wszystkie nowe/wszystkie istniejące jednostki</span><span class="sxs-lookup"><span data-stu-id="60e39-157">All new/all existing entities</span></span>

<span data-ttu-id="60e39-158">Przykładem pracy z wykresami jest wstawianie lub aktualizowanie bloga wraz z jego kolekcją skojarzonych wpisów.</span><span class="sxs-lookup"><span data-stu-id="60e39-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="60e39-159">Jeśli wszystkie jednostki na wykresie powinny być wstawione lub wszystkie powinny być aktualizowane, proces jest taki sam, jak opisano powyżej dla pojedynczych jednostek.</span><span class="sxs-lookup"><span data-stu-id="60e39-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="60e39-160">Na przykład wykres blogów i postów utworzonych w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="60e39-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="60e39-161">można wstawić w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="60e39-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="60e39-162">Wywołanie dodaj oznaczy bloga i wszystkie wpisy, które mają zostać wstawione.</span><span class="sxs-lookup"><span data-stu-id="60e39-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="60e39-163">Podobnie, jeśli wszystkie jednostki na wykresie muszą zostać zaktualizowane, można użyć aktualizacji:</span><span class="sxs-lookup"><span data-stu-id="60e39-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="60e39-164">Blog i wszystkie jego posty zostaną oznaczone jako zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="60e39-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="60e39-165">Połączenie nowych i istniejących podmiotów</span><span class="sxs-lookup"><span data-stu-id="60e39-165">Mix of new and existing entities</span></span>

<span data-ttu-id="60e39-166">W przypadku automatycznie generowanych kluczy aktualizacja może być ponownie używana zarówno dla wstawień, jak i aktualizacji, nawet jeśli wykres zawiera kombinację jednostek wymagających wstawiania i tych, które wymagają aktualizacji:</span><span class="sxs-lookup"><span data-stu-id="60e39-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="60e39-167">Aktualizacja oznaczy dowolną encję na wykresie, blogu lub poście do wstawienia, jeśli nie ma ustawionej wartości klucza, podczas gdy wszystkie inne jednostki są oznaczone do aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="60e39-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="60e39-168">Tak jak poprzednio, gdy nie używa się automatycznie generowanych kluczy, można użyć kwerendy i niektórych przetwarzania:</span><span class="sxs-lookup"><span data-stu-id="60e39-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="60e39-169">Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="60e39-169">Handling deletes</span></span>

<span data-ttu-id="60e39-170">Usuwanie może być trudne do obsługi, ponieważ często brak jednostki oznacza, że powinny zostać usunięte.</span><span class="sxs-lookup"><span data-stu-id="60e39-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="60e39-171">Jednym ze sposobów radzenia sobie z tym jest użycie "nietrwałych usuwania", tak aby jednostka była oznaczona jako usunięta, a nie faktycznie usuwana.</span><span class="sxs-lookup"><span data-stu-id="60e39-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="60e39-172">Usuwa następnie staje się taka sama jak aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="60e39-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="60e39-173">Usuwanie nietrwałe można zaimplementować przy użyciu [filtrów zapytań](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="60e39-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="60e39-174">W przypadku usuwania true wspólnego wzorca jest użycie rozszerzenia wzorca kwerendy do wykonywania tego, co jest zasadniczo różnicy graf.</span><span class="sxs-lookup"><span data-stu-id="60e39-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff.</span></span> <span data-ttu-id="60e39-175">Przykład:</span><span class="sxs-lookup"><span data-stu-id="60e39-175">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="60e39-176">Wykres dresowy</span><span class="sxs-lookup"><span data-stu-id="60e39-176">TrackGraph</span></span>

<span data-ttu-id="60e39-177">Wewnętrznie, Dodaj, Dołącz i Aktualizuj użyj wykresu przechodzenie z określeniem dla każdej jednostki, czy powinien być oznaczony jako Dodane (do wstawiania), Zmodyfikowane (do aktualizacji), Bez zmian (nic nie robić) lub usunięte (do usunięcia).</span><span class="sxs-lookup"><span data-stu-id="60e39-177">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="60e39-178">Ten mechanizm jest narażony za pośrednictwem interfejsu API trackgraph.</span><span class="sxs-lookup"><span data-stu-id="60e39-178">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="60e39-179">Załóżmy na przykład, że gdy klient odsyła wykres jednostek ustawia niektóre flagi na każdej jednostce wskazując, jak należy obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="60e39-179">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="60e39-180">TrackGraph może następnie służyć do przetwarzania tej flagi:</span><span class="sxs-lookup"><span data-stu-id="60e39-180">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="60e39-181">Flagi są wyświetlane tylko jako część jednostki dla uproszczenia przykładu.</span><span class="sxs-lookup"><span data-stu-id="60e39-181">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="60e39-182">Zazwyczaj flagi będą częścią DTO lub innego stanu uwzględnionego w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="60e39-182">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
