---
title: Operatory złożonych zapytań — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417744"
---
# <a name="complex-query-operators"></a><span data-ttu-id="50595-102">Złożone operatory zapytań</span><span class="sxs-lookup"><span data-stu-id="50595-102">Complex Query Operators</span></span>

<span data-ttu-id="50595-103">Zintegrowane zapytanie języka (LINQ) zawiera wiele złożonych operatorów, które łączą wiele źródeł danych lub wykonuje złożone przetwarzanie.</span><span class="sxs-lookup"><span data-stu-id="50595-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="50595-104">Nie wszystkie operatory LINQ mają odpowiednie tłumaczenia po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="50595-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="50595-105">Czasami kwerenda w jednym formularzu tłumaczy się na serwer, ale jeśli jest napisane w innej formie, nie tłumaczy się, nawet jeśli wynik jest taki sam.</span><span class="sxs-lookup"><span data-stu-id="50595-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="50595-106">Na tej stronie opisano niektóre operatorów złożonych i ich obsługiwane odmiany.</span><span class="sxs-lookup"><span data-stu-id="50595-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="50595-107">W przyszłych wydaniach możemy rozpoznać więcej wzorców i dodać ich odpowiednie tłumaczenia.</span><span class="sxs-lookup"><span data-stu-id="50595-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="50595-108">Ważne jest również, aby pamiętać, że obsługa tłumaczeń różni się między dostawcami.</span><span class="sxs-lookup"><span data-stu-id="50595-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="50595-109">Określone zapytanie, które jest tłumaczone w sqlserver, może nie działać dla baz danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="50595-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="50595-110">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="50595-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="50595-111">Join</span><span class="sxs-lookup"><span data-stu-id="50595-111">Join</span></span>

<span data-ttu-id="50595-112">Linq Join operator umożliwia połączenie dwóch źródeł danych na podstawie selektora kluczy dla każdego źródła, generowanie krotki wartości, gdy klucz pasuje.</span><span class="sxs-lookup"><span data-stu-id="50595-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="50595-113">To oczywiście przekłada `INNER JOIN` się na relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="50595-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="50595-114">Podczas gdy sprzężenie LINQ ma selektory kluczy zewnętrznych i wewnętrznych, baza danych wymaga pojedynczego warunku sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="50595-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="50595-115">Tak EF Core generuje warunek sprzężenia, porównując selektor klucza zewnętrznego do selektora klucza wewnętrznego dla równości.</span><span class="sxs-lookup"><span data-stu-id="50595-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="50595-116">Ponadto jeśli selektory kluczy są typami anonimowymi, EF Core generuje warunek sprzężenia, aby porównać składnik równości.</span><span class="sxs-lookup"><span data-stu-id="50595-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="50595-117">Groupjoin</span><span class="sxs-lookup"><span data-stu-id="50595-117">GroupJoin</span></span>

<span data-ttu-id="50595-118">Linq GroupJoin operator umożliwia połączenie dwóch źródeł danych podobnych do Sprzężenia, ale tworzy grupę wartości wewnętrznych do pasowania elementów zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="50595-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="50595-119">Wykonanie kwerendy, takiej jak w poniższym przykładzie, generuje wynik programu `Blog`  &  `IEnumerable<Post>`.</span><span class="sxs-lookup"><span data-stu-id="50595-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="50595-120">Ponieważ bazy danych (zwłaszcza relacyjnych baz danych) nie mają sposobu reprezentowania kolekcji obiektów po stronie klienta, GroupJoin nie tłumaczy się na serwer w wielu przypadkach.</span><span class="sxs-lookup"><span data-stu-id="50595-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="50595-121">Wymaga to, aby uzyskać wszystkie dane z serwera, aby zrobić GroupJoin bez specjalnego selektora (pierwsze zapytanie poniżej).</span><span class="sxs-lookup"><span data-stu-id="50595-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="50595-122">Ale jeśli selektor jest ograniczenie danych wybieranych następnie pobieranie wszystkich danych z serwera może spowodować problemy z wydajnością (drugie zapytanie poniżej).</span><span class="sxs-lookup"><span data-stu-id="50595-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="50595-123">Dlatego EF Core nie tłumaczy GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="50595-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="50595-124">Selectmany</span><span class="sxs-lookup"><span data-stu-id="50595-124">SelectMany</span></span>

<span data-ttu-id="50595-125">Linq SelectMany operator umożliwia wyliczenie za wiele selektor kolekcji dla każdego elementu zewnętrznego i generowania krotek wartości z każdego źródła danych.</span><span class="sxs-lookup"><span data-stu-id="50595-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="50595-126">W sposób, jest to sprzężenie, ale bez żadnych warunków, więc każdy element zewnętrzny jest połączony z elementem ze źródła kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50595-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="50595-127">W zależności od tego, jak selektor kolekcji jest powiązany z zewnętrznym źródłem danych, SelectMany może przełożyć się na różne zapytania po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="50595-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="50595-128">Selektor kolekcji nie odwołuje się do zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="50595-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="50595-129">Gdy selektor kolekcji nie odwołuje się do niczego z zewnętrznego źródła, wynik jest kartezjańskim produktem obu źródeł danych.</span><span class="sxs-lookup"><span data-stu-id="50595-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="50595-130">Przekłada się `CROSS JOIN` to na relacyjne bazy danych.</span><span class="sxs-lookup"><span data-stu-id="50595-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="50595-131">Selektor kolekcji odwołuje się zewnętrzny w klauzuli where</span><span class="sxs-lookup"><span data-stu-id="50595-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="50595-132">Gdy selektor kolekcji ma klauzuli where, która odwołuje się do elementu zewnętrznego, a następnie EF Core tłumaczy go do sprzężenia bazy danych i używa predykatu jako warunek sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="50595-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="50595-133">Zwykle ten przypadek występuje podczas korzystania z nawigacji kolekcji na zewnętrznym elemencie jako selektorze kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50595-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="50595-134">Jeśli kolekcja jest pusta dla elementu zewnętrznego, następnie nie wyniki zostaną wygenerowane dla tego elementu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="50595-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="50595-135">Ale `DefaultIfEmpty` jeśli jest stosowany na selektorze kolekcji następnie element zewnętrzny zostanie połączony z wartością domyślną elementu wewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="50595-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="50595-136">Z powodu tego rozróżnienia tego rodzaju `INNER JOIN` zapytań przekłada `DefaultIfEmpty` `LEFT JOIN` się `DefaultIfEmpty` na brak i kiedy są stosowane.</span><span class="sxs-lookup"><span data-stu-id="50595-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="50595-137">Selektor kolekcji odwołuje się do zewnętrznych w przypadku, gdy nie</span><span class="sxs-lookup"><span data-stu-id="50595-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="50595-138">Gdy selektor kolekcji odwołuje się do elementu zewnętrznego, który nie znajduje się w where klauzuli (jak w tym przypadku powyżej), nie przekłada się na sprzężenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="50595-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="50595-139">Dlatego musimy ocenić selektor kolekcji dla każdego elementu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="50595-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="50595-140">Przekłada się `APPLY` na operacje w wielu relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="50595-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="50595-141">Jeśli kolekcja jest pusta dla elementu zewnętrznego, następnie nie wyniki zostaną wygenerowane dla tego elementu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="50595-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="50595-142">Ale `DefaultIfEmpty` jeśli jest stosowany na selektorze kolekcji następnie element zewnętrzny zostanie połączony z wartością domyślną elementu wewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="50595-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="50595-143">Z powodu tego rozróżnienia tego rodzaju `CROSS APPLY` zapytań przekłada `DefaultIfEmpty` `OUTER APPLY` się `DefaultIfEmpty` na brak i kiedy są stosowane.</span><span class="sxs-lookup"><span data-stu-id="50595-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="50595-144">Niektóre bazy danych, takie jak `APPLY` SQLite nie obsługują operatorów, więc tego rodzaju zapytania nie mogą być tłumaczone.</span><span class="sxs-lookup"><span data-stu-id="50595-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="50595-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="50595-145">GroupBy</span></span>

<span data-ttu-id="50595-146">Linq GroupBy operatory utworzyć `IGrouping<TKey, TElement>` `TKey` wynik `TElement` typu, gdzie i może być dowolny typ.</span><span class="sxs-lookup"><span data-stu-id="50595-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="50595-147">Ponadto implementuje `IGrouping` `IEnumerable<TElement>`, co oznacza, że można skomponować nad nim za pomocą dowolnego operatora LINQ po grupowaniu.</span><span class="sxs-lookup"><span data-stu-id="50595-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="50595-148">Ponieważ żadna `IGrouping`struktura bazy danych nie może reprezentować operatorów GroupBy, które w większości przypadków nie mają tłumaczenia.</span><span class="sxs-lookup"><span data-stu-id="50595-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="50595-149">Gdy operator agregacji jest stosowany do każdej grupy, która zwraca skalarny, można go przetłumaczyć na język SQL `GROUP BY` w relacyjnych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="50595-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="50595-150">SQL `GROUP BY` jest również restrykcyjne.</span><span class="sxs-lookup"><span data-stu-id="50595-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="50595-151">Wymaga grupowanie tylko według wartości skalarnych.</span><span class="sxs-lookup"><span data-stu-id="50595-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="50595-152">Projekcja może zawierać tylko kolumny klucza grupowania lub dowolny agregacji zastosowane w kolumnie.</span><span class="sxs-lookup"><span data-stu-id="50595-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="50595-153">EF Core identyfikuje ten wzorzec i tłumaczy go na serwer, jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="50595-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="50595-154">EF Core tłumaczy również kwerendy, w których operator agregacji na grupowaniu pojawia się w Where lub OrderBy (lub innych zamawiania) LINQ operatora.</span><span class="sxs-lookup"><span data-stu-id="50595-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="50595-155">Używa klauzuli `HAVING` w języku SQL dla where klauzuli.</span><span class="sxs-lookup"><span data-stu-id="50595-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="50595-156">Część kwerendy przed zastosowaniem GroupBy operator może być wszelkie złożone zapytanie tak długo, jak można go przetłumaczyć na serwer.</span><span class="sxs-lookup"><span data-stu-id="50595-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="50595-157">Ponadto po zastosowaniu operatorów agregacji w kwerendzie grupowania w celu usunięcia grupowania z wynikowego źródła można skomponować na nim, jak każde inne zapytanie.</span><span class="sxs-lookup"><span data-stu-id="50595-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="50595-158">Operatory agregatów EF Core są następujące</span><span class="sxs-lookup"><span data-stu-id="50595-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="50595-159">Średnia</span><span class="sxs-lookup"><span data-stu-id="50595-159">Average</span></span>
- <span data-ttu-id="50595-160">Liczba</span><span class="sxs-lookup"><span data-stu-id="50595-160">Count</span></span>
- <span data-ttu-id="50595-161">Longcount</span><span class="sxs-lookup"><span data-stu-id="50595-161">LongCount</span></span>
- <span data-ttu-id="50595-162">Maks.</span><span class="sxs-lookup"><span data-stu-id="50595-162">Max</span></span>
- <span data-ttu-id="50595-163">Min.</span><span class="sxs-lookup"><span data-stu-id="50595-163">Min</span></span>
- <span data-ttu-id="50595-164">Suma</span><span class="sxs-lookup"><span data-stu-id="50595-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="50595-165">Lewe sprzężenie</span><span class="sxs-lookup"><span data-stu-id="50595-165">Left Join</span></span>

<span data-ttu-id="50595-166">Podczas gdy left join nie jest operatorem LINQ, relacyjne bazy danych mają pojęcie Left Join, który jest często używany w kwerendach.</span><span class="sxs-lookup"><span data-stu-id="50595-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="50595-167">Określony wzorzec w zapytaniach LINQ `LEFT JOIN` daje taki sam wynik jak na serwerze.</span><span class="sxs-lookup"><span data-stu-id="50595-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="50595-168">EF Core identyfikuje takie wzorce i `LEFT JOIN` generuje odpowiednik po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="50595-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="50595-169">Wzorzec polega na utworzeniu GroupJoin między źródłami danych, a następnie spłaszczenie grupowania przy użyciu SelectMany operatora z DefaultIfEmpty na źródle grupowania, aby dopasować null, gdy wewnętrzny nie ma elementu pokrewnego.</span><span class="sxs-lookup"><span data-stu-id="50595-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="50595-170">Poniższy przykład pokazuje, jak ten wzorzec wygląda i co generuje.</span><span class="sxs-lookup"><span data-stu-id="50595-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="50595-171">Powyższy wzorzec tworzy złożoną strukturę w drzewie wyrażeń.</span><span class="sxs-lookup"><span data-stu-id="50595-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="50595-172">Z tego powodu EF Core wymaga spłaszczenia wyników grupowania GroupJoin operatora w kroku bezpośrednio po operatora.</span><span class="sxs-lookup"><span data-stu-id="50595-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="50595-173">Nawet jeśli GroupJoin-DefaultIfEmpty-SelectMany jest używany, ale w innym wzorcu, nie możemy zidentyfikować go jako lewego sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="50595-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
