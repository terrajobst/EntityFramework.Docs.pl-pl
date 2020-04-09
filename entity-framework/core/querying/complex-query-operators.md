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
# <a name="complex-query-operators"></a>Złożone operatory zapytań

Zintegrowane zapytanie języka (LINQ) zawiera wiele złożonych operatorów, które łączą wiele źródeł danych lub wykonuje złożone przetwarzanie. Nie wszystkie operatory LINQ mają odpowiednie tłumaczenia po stronie serwera. Czasami kwerenda w jednym formularzu tłumaczy się na serwer, ale jeśli jest napisane w innej formie, nie tłumaczy się, nawet jeśli wynik jest taki sam. Na tej stronie opisano niektóre operatorów złożonych i ich obsługiwane odmiany. W przyszłych wydaniach możemy rozpoznać więcej wzorców i dodać ich odpowiednie tłumaczenia. Ważne jest również, aby pamiętać, że obsługa tłumaczeń różni się między dostawcami. Określone zapytanie, które jest tłumaczone w sqlserver, może nie działać dla baz danych SQLite.

> [!TIP]
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) przykład artykułu na GitHub.

## <a name="join"></a>Join

Linq Join operator umożliwia połączenie dwóch źródeł danych na podstawie selektora kluczy dla każdego źródła, generowanie krotki wartości, gdy klucz pasuje. To oczywiście przekłada `INNER JOIN` się na relacyjnych baz danych. Podczas gdy sprzężenie LINQ ma selektory kluczy zewnętrznych i wewnętrznych, baza danych wymaga pojedynczego warunku sprzężenia. Tak EF Core generuje warunek sprzężenia, porównując selektor klucza zewnętrznego do selektora klucza wewnętrznego dla równości. Ponadto jeśli selektory kluczy są typami anonimowymi, EF Core generuje warunek sprzężenia, aby porównać składnik równości.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>Groupjoin

Linq GroupJoin operator umożliwia połączenie dwóch źródeł danych podobnych do Sprzężenia, ale tworzy grupę wartości wewnętrznych do pasowania elementów zewnętrznych. Wykonanie kwerendy, takiej jak w poniższym przykładzie, generuje wynik programu `Blog`  &  `IEnumerable<Post>`. Ponieważ bazy danych (zwłaszcza relacyjnych baz danych) nie mają sposobu reprezentowania kolekcji obiektów po stronie klienta, GroupJoin nie tłumaczy się na serwer w wielu przypadkach. Wymaga to, aby uzyskać wszystkie dane z serwera, aby zrobić GroupJoin bez specjalnego selektora (pierwsze zapytanie poniżej). Ale jeśli selektor jest ograniczenie danych wybieranych następnie pobieranie wszystkich danych z serwera może spowodować problemy z wydajnością (drugie zapytanie poniżej). Dlatego EF Core nie tłumaczy GroupJoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>Selectmany

Linq SelectMany operator umożliwia wyliczenie za wiele selektor kolekcji dla każdego elementu zewnętrznego i generowania krotek wartości z każdego źródła danych. W sposób, jest to sprzężenie, ale bez żadnych warunków, więc każdy element zewnętrzny jest połączony z elementem ze źródła kolekcji. W zależności od tego, jak selektor kolekcji jest powiązany z zewnętrznym źródłem danych, SelectMany może przełożyć się na różne zapytania po stronie serwera.

### <a name="collection-selector-doesnt-reference-outer"></a>Selektor kolekcji nie odwołuje się do zewnętrznego

Gdy selektor kolekcji nie odwołuje się do niczego z zewnętrznego źródła, wynik jest kartezjańskim produktem obu źródeł danych. Przekłada się `CROSS JOIN` to na relacyjne bazy danych.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Selektor kolekcji odwołuje się zewnętrzny w klauzuli where

Gdy selektor kolekcji ma klauzuli where, która odwołuje się do elementu zewnętrznego, a następnie EF Core tłumaczy go do sprzężenia bazy danych i używa predykatu jako warunek sprzężenia. Zwykle ten przypadek występuje podczas korzystania z nawigacji kolekcji na zewnętrznym elemencie jako selektorze kolekcji. Jeśli kolekcja jest pusta dla elementu zewnętrznego, następnie nie wyniki zostaną wygenerowane dla tego elementu zewnętrznego. Ale `DefaultIfEmpty` jeśli jest stosowany na selektorze kolekcji następnie element zewnętrzny zostanie połączony z wartością domyślną elementu wewnętrznego. Z powodu tego rozróżnienia tego rodzaju `INNER JOIN` zapytań przekłada `DefaultIfEmpty` `LEFT JOIN` się `DefaultIfEmpty` na brak i kiedy są stosowane.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Selektor kolekcji odwołuje się do zewnętrznych w przypadku, gdy nie

Gdy selektor kolekcji odwołuje się do elementu zewnętrznego, który nie znajduje się w where klauzuli (jak w tym przypadku powyżej), nie przekłada się na sprzężenie bazy danych. Dlatego musimy ocenić selektor kolekcji dla każdego elementu zewnętrznego. Przekłada się `APPLY` na operacje w wielu relacyjnych baz danych. Jeśli kolekcja jest pusta dla elementu zewnętrznego, następnie nie wyniki zostaną wygenerowane dla tego elementu zewnętrznego. Ale `DefaultIfEmpty` jeśli jest stosowany na selektorze kolekcji następnie element zewnętrzny zostanie połączony z wartością domyślną elementu wewnętrznego. Z powodu tego rozróżnienia tego rodzaju `CROSS APPLY` zapytań przekłada `DefaultIfEmpty` `OUTER APPLY` się `DefaultIfEmpty` na brak i kiedy są stosowane. Niektóre bazy danych, takie jak `APPLY` SQLite nie obsługują operatorów, więc tego rodzaju zapytania nie mogą być tłumaczone.

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

Linq GroupBy operatory utworzyć `IGrouping<TKey, TElement>` `TKey` wynik `TElement` typu, gdzie i może być dowolny typ. Ponadto implementuje `IGrouping` `IEnumerable<TElement>`, co oznacza, że można skomponować nad nim za pomocą dowolnego operatora LINQ po grupowaniu. Ponieważ żadna `IGrouping`struktura bazy danych nie może reprezentować operatorów GroupBy, które w większości przypadków nie mają tłumaczenia. Gdy operator agregacji jest stosowany do każdej grupy, która zwraca skalarny, można go przetłumaczyć na język SQL `GROUP BY` w relacyjnych bazach danych. SQL `GROUP BY` jest również restrykcyjne. Wymaga grupowanie tylko według wartości skalarnych. Projekcja może zawierać tylko kolumny klucza grupowania lub dowolny agregacji zastosowane w kolumnie. EF Core identyfikuje ten wzorzec i tłumaczy go na serwer, jak w poniższym przykładzie:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

EF Core tłumaczy również kwerendy, w których operator agregacji na grupowaniu pojawia się w Where lub OrderBy (lub innych zamawiania) LINQ operatora. Używa klauzuli `HAVING` w języku SQL dla where klauzuli. Część kwerendy przed zastosowaniem GroupBy operator może być wszelkie złożone zapytanie tak długo, jak można go przetłumaczyć na serwer. Ponadto po zastosowaniu operatorów agregacji w kwerendzie grupowania w celu usunięcia grupowania z wynikowego źródła można skomponować na nim, jak każde inne zapytanie.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Operatory agregatów EF Core są następujące

- Średnia
- Liczba
- Longcount
- Maks.
- Min.
- Suma

## <a name="left-join"></a>Lewe sprzężenie

Podczas gdy left join nie jest operatorem LINQ, relacyjne bazy danych mają pojęcie Left Join, który jest często używany w kwerendach. Określony wzorzec w zapytaniach LINQ `LEFT JOIN` daje taki sam wynik jak na serwerze. EF Core identyfikuje takie wzorce i `LEFT JOIN` generuje odpowiednik po stronie serwera. Wzorzec polega na utworzeniu GroupJoin między źródłami danych, a następnie spłaszczenie grupowania przy użyciu SelectMany operatora z DefaultIfEmpty na źródle grupowania, aby dopasować null, gdy wewnętrzny nie ma elementu pokrewnego. Poniższy przykład pokazuje, jak ten wzorzec wygląda i co generuje.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Powyższy wzorzec tworzy złożoną strukturę w drzewie wyrażeń. Z tego powodu EF Core wymaga spłaszczenia wyników grupowania GroupJoin operatora w kroku bezpośrednio po operatora. Nawet jeśli GroupJoin-DefaultIfEmpty-SelectMany jest używany, ale w innym wzorcu, nie możemy zidentyfikować go jako lewego sprzężenia.
