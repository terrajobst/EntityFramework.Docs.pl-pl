---
title: Złożone operatory zapytań — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417744"
---
# <a name="complex-query-operators"></a>Złożone operatory zapytań

Program Query Integrated Language (LINQ) zawiera wiele operatorów złożonych, które łączą wiele źródeł danych lub przetwarzają złożone. Nie wszystkie operatory LINQ mają odpowiednie tłumaczenia po stronie serwera. Czasami zapytanie w jednym formularzu jest tłumaczone na serwer, ale jeśli zapisywana w innym formularzu nie zostanie przetłumaczyć nawet wtedy, gdy wynik jest taki sam. Na tej stronie opisano niektóre operatory złożone i ich obsługiwane odmiany. W przyszłych wersjach możemy rozpoznać więcej wzorców i dodać odpowiednie tłumaczenia. Należy również pamiętać, że obsługa tłumaczenia zależy od dostawców. Określone zapytanie, które jest tłumaczone w programie SqlServer, może nie współpracować z bazami danych oprogramowania SQLite.

> [!TIP]
> [Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="join"></a>Join

Operator sprzężenia LINQ umożliwia łączenie dwóch źródeł danych na podstawie selektora kluczy dla każdego źródła, generując spójną krotkę wartości, gdy klucz jest zgodny. Jest to naturalnie tłumaczone na `INNER JOIN` w relacyjnych bazach danych. Chociaż element LINQ Join ma selektory kluczy zewnętrznych i wewnętrznych, baza danych wymaga jednego warunku sprzężenia. Dlatego EF Core generuje warunek sprzężenia poprzez porównanie selektora klucza zewnętrznego z selektorem wewnętrznego klucza dla równości. Ponadto, jeśli selektory kluczy są typami anonimowymi, EF Core generuje warunek sprzężenia, aby porównać składnik równości.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin —

Operator LINQ GroupJoin — umożliwia łączenie dwóch źródeł danych podobnych do przyłączenia, ale tworzy grupę wartości wewnętrznych dla dopasowywania elementów zewnętrznych. Wykonanie zapytania, takiego jak Poniższy przykład generuje wynik `Blog` & `IEnumerable<Post>`. Ponieważ bazy danych (szczególnie relacyjne bazy danych) nie mają sposobu reprezentowania kolekcji obiektów po stronie klienta, GroupJoin — nie są tłumaczone na serwer w wielu przypadkach. Wymaga to pobrania wszystkich danych z serwera, aby wykonać GroupJoin — bez specjalnego selektora (pierwsze zapytanie poniżej). Jeśli jednak selektor ogranicza wybrane dane, pobranie wszystkich danych z serwera może spowodować problemy z wydajnością (drugie zapytanie poniżej). Dlatego EF Core nie tłumaczy GroupJoin —.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

Operator LINQ SelectMany umożliwia Wyliczenie selektorów kolekcji dla każdego elementu zewnętrznego i generowanie spójnych krotek wartości z każdego źródła danych. W taki sposób jest to sprzężenie, ale bez żadnego warunku, tak aby każdy element zewnętrzny był połączony z elementem ze źródła kolekcji. W zależności od tego, jak selektor kolekcji jest powiązany z zewnętrznym źródłem danych, SelectMany może przełożyć na różne zapytania po stronie serwera.

### <a name="collection-selector-doesnt-reference-outer"></a>Selektor kolekcji nie odwołuje się do parametru Outer

Gdy selektor kolekcji nie odwołuje się do niczego ze źródła zewnętrznego, wynik jest produktem kartezjańskiego obu źródeł danych. Jest ona tłumaczona na `CROSS JOIN` w relacyjnych bazach danych.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Selektor kolekcji odwołuje się do elementu zewnętrznego w klauzuli WHERE

Gdy selektor kolekcji ma klauzulę WHERE, która odwołuje się do elementu zewnętrznego, a następnie EF Core przetłumaczy ją na sprzężenie bazy danych i używa predykatu jako warunku sprzężenia. Zwykle jest to spowodowane użyciem nawigacji kolekcji na zewnętrznym elemencie jako selektor kolekcji. Jeśli kolekcja jest pusta dla elementu zewnętrznego, nie będą generowane żadne wyniki dla tego elementu zewnętrznego. Ale jeśli `DefaultIfEmpty` jest zastosowany w selektorze kolekcji, zewnętrzny element zostanie połączony z domyślną wartością elementu wewnętrznego. Z powodu tego rozróżnienia tego rodzaju zapytania są tłumaczone na `INNER JOIN` w przypadku braku `DefaultIfEmpty` i `LEFT JOIN` w przypadku zastosowania `DefaultIfEmpty`.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Selektor kolekcji odwołuje się do elementu zewnętrznego w przypadku braku miejsca

Gdy selektor kolekcji odwołuje się do elementu zewnętrznego, który nie znajduje się w klauzuli WHERE (jak powyżej), nie jest przekształcany na sprzężenie bazy danych. Dlatego musimy oszacować selektor kolekcji dla każdego elementu zewnętrznego. Tłumaczy do operacji `APPLY` w wielu relacyjnych bazach danych. Jeśli kolekcja jest pusta dla elementu zewnętrznego, nie będą generowane żadne wyniki dla tego elementu zewnętrznego. Ale jeśli `DefaultIfEmpty` jest zastosowany w selektorze kolekcji, zewnętrzny element zostanie połączony z domyślną wartością elementu wewnętrznego. Z powodu tego rozróżnienia tego rodzaju zapytania są tłumaczone na `CROSS APPLY` w przypadku braku `DefaultIfEmpty` i `OUTER APPLY` w przypadku zastosowania `DefaultIfEmpty`. Niektóre bazy danych, takie jak SQLite, nie obsługują operatorów `APPLY`, więc tego rodzaju zapytania nie można przetłumaczyć.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

Operatory LINQ GroupBy tworzą wynik typu `IGrouping<TKey, TElement>`, gdzie `TKey` i `TElement` może być dowolnego dowolnego typu. Ponadto `IGrouping` implementuje `IEnumerable<TElement>`, co oznacza, że można je redagować przy użyciu dowolnego operatora LINQ po grupowaniu. Ponieważ struktura bazy danych nie może reprezentować `IGrouping`, w większości przypadków operatory GroupBy nie mają żadnego tłumaczenia. Gdy operator zagregowany jest stosowany do każdej grupy, która zwraca wartość skalarną, można ją przetłumaczyć na `GROUP BY` SQL w relacyjnych bazach danych. `GROUP BY` SQL jest restrykcyjna. Wymaga grupowania tylko wartości skalarnych. Projekcja może zawierać tylko kolumny klucza grupowania lub agregację zastosowana w kolumnie. EF Core identyfikuje ten wzorzec i tłumaczy go na serwer, tak jak w poniższym przykładzie:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core tłumaczy również zapytania, w których operator agregacji w grupowaniu pojawia się w operatorze "Where" lub "OrderBy" lub innej kolejności. Używa w języku SQL klauzul `HAVING` w przypadku klauzuli WHERE. Część zapytania przed zastosowaniem operatora GroupBy może być dowolną złożoną kwerendą, o ile można ją przetłumaczyć na serwer. Ponadto po zastosowaniu operatorów agregujących w zapytaniu grupowania, aby usunąć grupowanie z wyników źródła, można utworzyć je na początku, podobnie jak w przypadku innych zapytań.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Operatory agregujące EF Core obsługiwane są następujące czynności:

- Średnia
- Licznik
- LongCount
- Maks.
- Min.
- Suma

## <a name="left-join"></a>Lewe sprzężenie

Podczas gdy lewe sprzężenie nie jest operatorem LINQ, relacyjne bazy danych mają koncepcję lewego sprzężenia, które jest często używane w zapytaniach. Określony wzorzec zapytań LINQ daje ten sam wynik jako `LEFT JOIN` na serwerze. EF Core identyfikuje takie wzorce i generuje odpowiedni `LEFT JOIN` po stronie serwera. Wzorzec polega na utworzeniu GroupJoin — między źródłami danych a następnie spłaszczeniem grupy przy użyciu operatora SelectMany z DefaultIfEmpty w źródle grupowania, aby dopasować wartość null, gdy wewnętrzna nie ma powiązanego elementu. Poniższy przykład pokazuje, jak wygląda ten wzorzec i co generuje.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Powyższy wzorzec tworzy złożoną strukturę w drzewie wyrażenia. Z tego powodu EF Core wymaga, aby spłaszczyć wyniki grupowania operatora GroupJoin — w kroku bezpośrednio po operatorze. Nawet jeśli GroupJoin —-DefaultIfEmpty-SelectMany jest używany, ale w innym wzorcu, firma Microsoft może nie identyfikować jej jako lewego sprzężenia.
