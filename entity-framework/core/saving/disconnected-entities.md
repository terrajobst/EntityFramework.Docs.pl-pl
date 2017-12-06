---
title: "Odłączonych jednostek - EF Core"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="2e614-102">Odłączonych jednostek.</span><span class="sxs-lookup"><span data-stu-id="2e614-102">Disconnected entities</span></span>

<span data-ttu-id="2e614-103">Wystąpienie typu DbContext automatycznie śledzi zwróconych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2e614-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="2e614-104">Zmiany wprowadzone do tych jednostek następnie zostanie wykryty, gdy jest wywoływana SaveChanges i bazy danych zostanie zaktualizowany zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="2e614-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="2e614-105">Zobacz [podstawowe zapisać](basic.md) i [dane dotyczące](related-data.md) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="2e614-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="2e614-106">Jednak czasami jednostek będą pytani, przy użyciu jednego wystąpienia kontekstu, a następnie zapisane przy użyciu innego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="2e614-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="2e614-107">Odbywa się to często w "odłączony" scenariuszy, takich jak aplikacji sieci web, w którym jednostek są proszeni, wysyłane do klienta zmodyfikowane, wysyłane z powrotem do serwera w żądaniu i zapisany.</span><span class="sxs-lookup"><span data-stu-id="2e614-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="2e614-108">W takim przypadku kontekst drugiego wystąpienia musi wiedzieć, czy jednostki są nowe (powinny zostać wstawione) lub istniejące (powinien być zaktualizowany).</span><span class="sxs-lookup"><span data-stu-id="2e614-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="2e614-109">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2e614-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="2e614-110">Identyfikowanie nowych jednostek</span><span class="sxs-lookup"><span data-stu-id="2e614-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="2e614-111">Klient identyfikuje nowe jednostki</span><span class="sxs-lookup"><span data-stu-id="2e614-111">Client identifies new entities</span></span>

<span data-ttu-id="2e614-112">Najprostszym przypadku radzenia sobie z jest, gdy klient informuje serwer, czy jednostka jest nowy lub istniejący.</span><span class="sxs-lookup"><span data-stu-id="2e614-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="2e614-113">Na przykład często żądania, aby wstawić nową jednostkę różni się od żądania w celu zaktualizowania istniejącej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2e614-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="2e614-114">W pozostałej części tej sekcji omówiono przypadkach gdy niezbędne do określenia w inny sposób, czy można wstawić ani zaktualizować go.</span><span class="sxs-lookup"><span data-stu-id="2e614-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="2e614-115">Z automatycznego generowania kluczy</span><span class="sxs-lookup"><span data-stu-id="2e614-115">With auto-generated keys</span></span>

<span data-ttu-id="2e614-116">Wartość klucza automatycznie generowanych często może służyć do określenia, czy jednostka musi być wstawiane lub aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2e614-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="2e614-117">Jeśli klucz nie został ustawiony, (tj. nadal posiada CLR domyślna wartość null, zero, itp.), jednostki musi być nowy i wymaga Wstawianie.</span><span class="sxs-lookup"><span data-stu-id="2e614-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="2e614-118">Z drugiej strony Jeśli ustawiono wartość tego klucza, następnie je musi już zostały wcześniej zapisane i teraz konieczna jest aktualizacja.</span><span class="sxs-lookup"><span data-stu-id="2e614-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="2e614-119">Innymi słowy Jeśli klucz ma wartość, następnie jednostki kwerendy, wysyłane do klienta i ma teraz wróć do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="2e614-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="2e614-120">Jest łatwy do sprawdzenia nie ustawiono klucza, gdy jest znany typ jednostki:</span><span class="sxs-lookup"><span data-stu-id="2e614-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="2e614-121">EF ma również wbudowane sposób w tym celu dla typu jednostki, a typ klucza:</span><span class="sxs-lookup"><span data-stu-id="2e614-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="2e614-122">Klucze są ustawiane jak jednostki są śledzone przez kontekst, nawet jeśli obiekt jest w stanie Added.</span><span class="sxs-lookup"><span data-stu-id="2e614-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="2e614-123">Dzięki temu podczas przesyłania wykres jednostek i decydowanie o każdym, takich jak przy użyciu interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="2e614-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="2e614-124">Wartość klucza należy używać tylko w taki sposób, w tym miejscu pokazano _przed_ żadnych wywołanie do śledzenia jednostki.</span><span class="sxs-lookup"><span data-stu-id="2e614-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="2e614-125">Z kluczy</span><span class="sxs-lookup"><span data-stu-id="2e614-125">With other keys</span></span>

<span data-ttu-id="2e614-126">Niektóre inny mechanizm jest potrzebna do nowych jednostek tożsamości, gdy wartości klucza nie są generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="2e614-126">Some other mechanism is needed to identity new entities when key values are not generated automatically.</span></span> <span data-ttu-id="2e614-127">Istnieją dwie metody ogólne do poniższego:</span><span class="sxs-lookup"><span data-stu-id="2e614-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="2e614-128">Zapytanie dla jednostki</span><span class="sxs-lookup"><span data-stu-id="2e614-128">Query for the entity</span></span>
 * <span data-ttu-id="2e614-129">Przekaż flagę z klienta</span><span class="sxs-lookup"><span data-stu-id="2e614-129">Pass a flag from the client</span></span>

<span data-ttu-id="2e614-130">Dla obiekt kwerendy, po prostu użyj metody Find:</span><span class="sxs-lookup"><span data-stu-id="2e614-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="2e614-131">Wykracza poza zakres tego dokumentu, aby wyświetlić pełny kod może być przekazywany flagę od klienta.</span><span class="sxs-lookup"><span data-stu-id="2e614-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="2e614-132">W aplikacji sieci web zwykle oznacza to, wysyłania żądań różne dla różnych działań, lub przekazywanie niektórych stanu w żądaniu, a następnie wyodrębnij go w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="2e614-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="2e614-133">Zapisywanie pojedynczych jednostek</span><span class="sxs-lookup"><span data-stu-id="2e614-133">Saving single entities</span></span>

<span data-ttu-id="2e614-134">Jeśli wiadomo, czy jest konieczne insert lub update, a następnie dodaj lub zaktualizuj można odpowiednio:</span><span class="sxs-lookup"><span data-stu-id="2e614-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="2e614-135">Jednak jeśli jednostka używa automatycznego generowania wartości klucza, następnie metody aktualizacji można użyć w obu przypadkach:</span><span class="sxs-lookup"><span data-stu-id="2e614-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="2e614-136">Metody aktualizacji zwykle oznacza jednostki aktualizacji nie insert.</span><span class="sxs-lookup"><span data-stu-id="2e614-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="2e614-137">Jednak jeśli jednostka ma klucz wygenerowany automatycznie, a nie wartość klucza nie ustawiono, jednostki zamiast tego jest automatycznie oznaczone do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="2e614-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="2e614-138">To zachowanie została wprowadzona w programie EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2e614-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="2e614-139">W starszych wersjach należy zawsze jawnie wybierz polecenie Dodaj lub zaktualizuj.</span><span class="sxs-lookup"><span data-stu-id="2e614-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="2e614-140">Jeśli jednostka nie używa automatycznego generowania kluczy, a następnie aplikacji należy zdecydować, czy jednostka powinna być wstawiane lub aktualizowane: na przykład:</span><span class="sxs-lookup"><span data-stu-id="2e614-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="2e614-141">Czynności opisane w tym miejscu są:</span><span class="sxs-lookup"><span data-stu-id="2e614-141">The steps here are:</span></span>
* <span data-ttu-id="2e614-142">Dodaj Znajdź zwraca wartość null, a następnie bazy danych nie zawiera już blog o tym identyfikatorze, dlatego nazywamy Oznacz ją do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="2e614-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="2e614-143">Jeśli Znajdź zwraca jednostkę, następnie istnieje w bazie danych i kontekst teraz śledzi istniejącej jednostki</span><span class="sxs-lookup"><span data-stu-id="2e614-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="2e614-144">Następnie używamy SetValues można ustawić wartości dla wszystkich właściwości dla tej jednostki do tych, które pochodzą od klienta.</span><span class="sxs-lookup"><span data-stu-id="2e614-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="2e614-145">Wywołanie SetValues oznaczy jednostka do zaktualizowania zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="2e614-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="2e614-146">SetValues oznaczy tylko modyfikowanie właściwości, które mają różne wartości określone w śledzonych jednostki.</span><span class="sxs-lookup"><span data-stu-id="2e614-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="2e614-147">Oznacza to, że po wysłaniu aktualizacji, tylko te kolumny, które faktycznie zostały zmienione zostaną zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="2e614-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="2e614-148">(I nic się nie zmieniło, aktualizacja nie zostanie wysłana na wszystkich.)</span><span class="sxs-lookup"><span data-stu-id="2e614-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="2e614-149">Praca z wykresów</span><span class="sxs-lookup"><span data-stu-id="2e614-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="2e614-150">Wszystkie nowe/wszystkich istniejących obiektów.</span><span class="sxs-lookup"><span data-stu-id="2e614-150">All new/all existing entities</span></span>

<span data-ttu-id="2e614-151">Przykład pracy wykresami jest wstawiania lub aktualizowania blogu wraz z jego kolekcja wpisów skojarzone.</span><span class="sxs-lookup"><span data-stu-id="2e614-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="2e614-152">Jeśli wszystkie jednostki na wykresie powinny zostać wstawione lub wszystkie powinien zostać zaktualizowany, następnie proces jest taka sama, jak opisano powyżej dla jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2e614-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="2e614-153">Na przykład wykres blogów i wpisy utworzone w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2e614-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="2e614-154">mogą być wstawiane następująco:</span><span class="sxs-lookup"><span data-stu-id="2e614-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="2e614-155">Wywołania do Add oznaczy blogu i wszystkie wpisy do wstawienia.</span><span class="sxs-lookup"><span data-stu-id="2e614-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="2e614-156">Podobnie jeśli wszystkie jednostki na wykresie muszą zostać zaktualizowane, następnie aktualizacja można:</span><span class="sxs-lookup"><span data-stu-id="2e614-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="2e614-157">Blog i wszystkie jego wpisy zostaną oznaczone do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="2e614-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="2e614-158">Mieszane nowych i istniejących jednostek</span><span class="sxs-lookup"><span data-stu-id="2e614-158">Mix of new and existing entities</span></span>

<span data-ttu-id="2e614-159">Z automatycznego generowania kluczy aktualizacji ponownie można dla operacji wstawiania i aktualizacji, nawet jeśli wykres zawiera różnych jednostek, które wymagają Wstawianie oraz te, które wymagają zaktualizowania:</span><span class="sxs-lookup"><span data-stu-id="2e614-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="2e614-160">Aktualizacja spowoduje oznaczenie dowolnej jednostki na wykresie, blog lub post przez operację wstawiania, jeśli nie ma ustawioną wartość klucza, podczas gdy inne jednostki są oznaczone dla aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="2e614-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="2e614-161">Jako przed, gdy nie używa automatycznego generowania kluczy, kwerendy i przetwarza mogą być używane:</span><span class="sxs-lookup"><span data-stu-id="2e614-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="2e614-162">Obsługa usuwa</span><span class="sxs-lookup"><span data-stu-id="2e614-162">Handling deletes</span></span>

<span data-ttu-id="2e614-163">Delete mogą być trudnych do obsługi, ponieważ często Brak jednostki oznacza, że należy ją usunąć.</span><span class="sxs-lookup"><span data-stu-id="2e614-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="2e614-164">Jednym ze sposobów postępowania w przypadku to jest do użycia "usuwania nietrwałego" w taki sposób, że jednostka jest oznaczone jako usunięte zamiast faktycznie usunięte.</span><span class="sxs-lookup"><span data-stu-id="2e614-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="2e614-165">Usuwa, a następnie staje się taka sama jak aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="2e614-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="2e614-166">Elastyczne usuwa można zaimplementować przy użyciu [zapytania filtry](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="2e614-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="2e614-167">Dla usunięć wartość true jest użycie rozszerzenia wzorca zapytania do wykonania, co to jest zasadniczo różn. wykres wspólnego wzorca Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2e614-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="2e614-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="2e614-168">TrackGraph</span></span>

<span data-ttu-id="2e614-169">Wewnętrznie, Dodaj Attach i aktualizacji za pomocą wykresu przechodzenie określenie wprowadzone dla każdej jednostki określające, czy jego powinien być oznaczony jako Added (Aby wstawić), zmodyfikowany (w celu aktualizacji), Unchanged (nic nie rób), lub usunięte (Aby usunąć).</span><span class="sxs-lookup"><span data-stu-id="2e614-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="2e614-170">Ten mechanizm jest uwidaczniany za pomocą interfejsu API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="2e614-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="2e614-171">Na przykład załóżmy, że że gdy klient przesyła wykres jednostek ustawia niektóre flagi dla każdej jednostki wskazującą sposób obsługi.</span><span class="sxs-lookup"><span data-stu-id="2e614-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="2e614-172">Następnie można TrackGraph do przetworzenia tej flagi:</span><span class="sxs-lookup"><span data-stu-id="2e614-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="2e614-173">Flagi są wyświetlane tylko w ramach jednostki dla uproszczenia przykładu.</span><span class="sxs-lookup"><span data-stu-id="2e614-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="2e614-174">Zazwyczaj flagi powinien być częścią DTO lub inny stan zawarte w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="2e614-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
