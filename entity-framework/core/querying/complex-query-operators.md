---
title: Złożone operatory zapytań — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 350a7fa6a3ee1de16bad4b63e10842f9356a1b60
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186261"
---
# <a name="complex-query-operators"></a><span data-ttu-id="1e4b8-102">Złożone operatory zapytań</span><span class="sxs-lookup"><span data-stu-id="1e4b8-102">Complex Query Operators</span></span>

<span data-ttu-id="1e4b8-103">Program Query Integrated Language (LINQ) zawiera wiele operatorów złożonych, które łączą wiele źródeł danych lub przetwarzają złożone.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="1e4b8-104">Nie wszystkie operatory LINQ mają odpowiednie tłumaczenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="1e4b8-105">Czasami zapytanie w jednym formularzu jest tłumaczone na serwer, ale jeśli zapisywana w innym formularzu nie zostanie przetłumaczyć nawet wtedy, gdy wynik jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="1e4b8-106">Na tej stronie opisano niektóre operatory złożone i ich obsługiwane odmiany.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="1e4b8-107">W przyszłych wersjach możemy rozpoznać więcej wzorców i dodać odpowiednie tłumaczenia.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="1e4b8-108">Należy również pamiętać, że obsługa tłumaczenia zależy od dostawców.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="1e4b8-109">Określone zapytanie, które jest tłumaczone w programie SqlServer, może nie współpracować z bazami danych oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="1e4b8-110">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-110">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="1e4b8-111">Join</span><span class="sxs-lookup"><span data-stu-id="1e4b8-111">Join</span></span>

<span data-ttu-id="1e4b8-112">Operator sprzężenia LINQ umożliwia łączenie dwóch źródeł danych na podstawie selektora kluczy dla każdego źródła, generując spójną krotkę wartości, gdy klucz jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="1e4b8-113">Jest to naturalnie tłumaczone na `INNER JOIN` w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="1e4b8-114">Chociaż element LINQ Join ma selektory kluczy zewnętrznych i wewnętrznych, baza danych wymaga jednego warunku sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="1e4b8-115">Dlatego EF Core generuje warunek sprzężenia poprzez porównanie selektora klucza zewnętrznego z selektorem wewnętrznego klucza dla równości.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="1e4b8-116">Ponadto, jeśli selektory kluczy są typami anonimowymi, EF Core generuje warunek sprzężenia, aby porównać składnik równości.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="1e4b8-117">GroupJoin —</span><span class="sxs-lookup"><span data-stu-id="1e4b8-117">GroupJoin</span></span>

<span data-ttu-id="1e4b8-118">Operator LINQ GroupJoin — umożliwia łączenie dwóch źródeł danych podobnych do przyłączenia, ale tworzy grupę wartości wewnętrznych dla dopasowywania elementów zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="1e4b8-119">Wykonanie zapytania, takiego jak Poniższy przykład generuje wynik `Blog` & `IEnumerable<Post>`.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="1e4b8-120">Ponieważ bazy danych (szczególnie relacyjne bazy danych) nie mają sposobu reprezentowania kolekcji obiektów po stronie klienta, GroupJoin — nie są tłumaczone na serwer w wielu przypadkach.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="1e4b8-121">Wymaga to pobrania wszystkich danych z serwera, aby wykonać GroupJoin — bez specjalnego selektora (pierwsze zapytanie poniżej).</span><span class="sxs-lookup"><span data-stu-id="1e4b8-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="1e4b8-122">Jeśli jednak selektor ogranicza wybrane dane, pobranie wszystkich danych z serwera może spowodować problemy z wydajnością (drugie zapytanie poniżej).</span><span class="sxs-lookup"><span data-stu-id="1e4b8-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="1e4b8-123">Dlatego EF Core nie tłumaczy GroupJoin —.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="1e4b8-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="1e4b8-124">SelectMany</span></span>

<span data-ttu-id="1e4b8-125">Operator LINQ SelectMany umożliwia Wyliczenie selektorów kolekcji dla każdego elementu zewnętrznego i generowanie spójnych krotek wartości z każdego źródła danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="1e4b8-126">W taki sposób jest to sprzężenie, ale bez żadnego warunku, tak aby każdy element zewnętrzny był połączony z elementem ze źródła kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="1e4b8-127">W zależności od tego, jak selektor kolekcji jest powiązany z zewnętrznym źródłem danych, SelectMany może przełożyć na różne zapytania po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="1e4b8-128">Selektor kolekcji nie odwołuje się do parametru Outer</span><span class="sxs-lookup"><span data-stu-id="1e4b8-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="1e4b8-129">Gdy selektor kolekcji nie odwołuje się do niczego ze źródła zewnętrznego, wynik jest produktem kartezjańskiego obu źródeł danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="1e4b8-130">Jest ona tłumaczona na `CROSS JOIN` w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="1e4b8-131">Selektor kolekcji odwołuje się do elementu zewnętrznego w klauzuli WHERE</span><span class="sxs-lookup"><span data-stu-id="1e4b8-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="1e4b8-132">Gdy selektor kolekcji ma klauzulę WHERE, która odwołuje się do elementu zewnętrznego, a następnie EF Core przetłumaczy ją na sprzężenie bazy danych i używa predykatu jako warunku sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="1e4b8-133">Zwykle jest to spowodowane użyciem nawigacji kolekcji na zewnętrznym elemencie jako selektor kolekcji.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="1e4b8-134">Jeśli kolekcja jest pusta dla elementu zewnętrznego, nie będą generowane żadne wyniki dla tego elementu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="1e4b8-135">Ale jeśli `DefaultIfEmpty` jest zastosowany w selektorze kolekcji, zewnętrzny element zostanie połączony z domyślną wartością elementu wewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="1e4b8-136">Z powodu tego rozróżnienia tego rodzaju zapytania są tłumaczone na `INNER JOIN` w przypadku braku `DefaultIfEmpty` i `LEFT JOIN` w przypadku zastosowania `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="1e4b8-137">Selektor kolekcji odwołuje się do elementu zewnętrznego w przypadku braku miejsca</span><span class="sxs-lookup"><span data-stu-id="1e4b8-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="1e4b8-138">Gdy selektor kolekcji odwołuje się do elementu zewnętrznego, który nie znajduje się w klauzuli WHERE (jak powyżej), nie jest przekształcany na sprzężenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="1e4b8-139">Dlatego musimy oszacować selektor kolekcji dla każdego elementu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="1e4b8-140">Tłumaczy do operacji `APPLY` w wielu relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="1e4b8-141">Jeśli kolekcja jest pusta dla elementu zewnętrznego, nie będą generowane żadne wyniki dla tego elementu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="1e4b8-142">Ale jeśli `DefaultIfEmpty` jest zastosowany w selektorze kolekcji, zewnętrzny element zostanie połączony z domyślną wartością elementu wewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="1e4b8-143">Z powodu tego rozróżnienia tego rodzaju zapytania są tłumaczone na `CROSS APPLY` w przypadku braku `DefaultIfEmpty` i `OUTER APPLY` w przypadku zastosowania `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="1e4b8-144">Niektóre bazy danych, takie jak SQLite, nie obsługują operatorów `APPLY`, więc tego rodzaju zapytania nie można przetłumaczyć.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="1e4b8-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="1e4b8-145">GroupBy</span></span>

<span data-ttu-id="1e4b8-146">Operatory LINQ GroupBy tworzą wynik typu `IGrouping<TKey, TElement>`, gdzie `TKey` i `TElement` może być dowolnego dowolnego typu.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="1e4b8-147">Ponadto `IGrouping` implementuje `IEnumerable<TElement>`, co oznacza, że można je redagować przy użyciu dowolnego operatora LINQ po grupowaniu.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="1e4b8-148">Ponieważ struktura bazy danych nie może reprezentować `IGrouping`, w większości przypadków operatory GroupBy nie mają żadnego tłumaczenia.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="1e4b8-149">Gdy operator zagregowany jest stosowany do każdej grupy, która zwraca wartość skalarną, można ją przetłumaczyć na `GROUP BY` SQL w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="1e4b8-150">`GROUP BY` SQL jest restrykcyjna.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="1e4b8-151">Wymaga grupowania tylko wartości skalarnych.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="1e4b8-152">Projekcja może zawierać tylko kolumny klucza grupowania lub agregację zastosowana w kolumnie.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="1e4b8-153">EF Core identyfikuje ten wzorzec i tłumaczy go na serwer, tak jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="1e4b8-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="1e4b8-154">EF Core tłumaczy również zapytania, w których operator agregacji w grupowaniu pojawia się w operatorze "Where" lub "OrderBy" lub innej kolejności.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="1e4b8-155">Używa w języku SQL klauzul `HAVING` w przypadku klauzuli WHERE.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="1e4b8-156">Część zapytania przed zastosowaniem operatora GroupBy może być dowolną złożoną kwerendą, o ile można ją przetłumaczyć na serwer.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="1e4b8-157">Ponadto po zastosowaniu operatorów agregujących w zapytaniu grupowania, aby usunąć grupowanie z wyników źródła, można utworzyć je na początku, podobnie jak w przypadku innych zapytań.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="1e4b8-158">Operatory agregujące EF Core obsługiwane są następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1e4b8-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="1e4b8-159">Średnia</span><span class="sxs-lookup"><span data-stu-id="1e4b8-159">Average</span></span>
- <span data-ttu-id="1e4b8-160">Licznik</span><span class="sxs-lookup"><span data-stu-id="1e4b8-160">Count</span></span>
- <span data-ttu-id="1e4b8-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="1e4b8-161">LongCount</span></span>
- <span data-ttu-id="1e4b8-162">Maks.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-162">Max</span></span>
- <span data-ttu-id="1e4b8-163">Min.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-163">Min</span></span>
- <span data-ttu-id="1e4b8-164">Suma</span><span class="sxs-lookup"><span data-stu-id="1e4b8-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="1e4b8-165">Lewe sprzężenie</span><span class="sxs-lookup"><span data-stu-id="1e4b8-165">Left Join</span></span>

<span data-ttu-id="1e4b8-166">Podczas gdy lewe sprzężenie nie jest operatorem LINQ, relacyjne bazy danych mają koncepcję lewego sprzężenia, które jest często używane w zapytaniach.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="1e4b8-167">Określony wzorzec zapytań LINQ daje ten sam wynik jako `LEFT JOIN` na serwerze.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="1e4b8-168">EF Core identyfikuje takie wzorce i generuje odpowiedni `LEFT JOIN` po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="1e4b8-169">Wzorzec polega na utworzeniu GroupJoin — między źródłami danych a następnie spłaszczeniem grupy przy użyciu operatora SelectMany z DefaultIfEmpty w źródle grupowania, aby dopasować wartość null, gdy wewnętrzna nie ma powiązanego elementu.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="1e4b8-170">Poniższy przykład pokazuje, jak wygląda ten wzorzec i co generuje.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="1e4b8-171">Powyższy wzorzec tworzy złożoną strukturę w drzewie wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="1e4b8-172">Z tego powodu EF Core wymaga, aby spłaszczyć wyniki grupowania operatora GroupJoin — w kroku bezpośrednio po operatorze.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="1e4b8-173">Nawet jeśli GroupJoin —-DefaultIfEmpty-SelectMany jest używany, ale w innym wzorcu, firma Microsoft może nie identyfikować jej jako lewego sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="1e4b8-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
