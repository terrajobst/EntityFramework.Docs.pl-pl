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
# <a name="disconnected-entities"></a>Odłączone encje

Wystąpienie DbContext będzie automatycznie śledzić jednostki zwrócone z bazy danych. Zmiany wprowadzone w tych jednostkach zostaną wykryte po wywołaniu savechanges i aktualizacji bazy danych w razie potrzeby. Szczegółowe informacje można znaleźć w [podstawowych danych zapisu](basic.md) i [powiązanych danych.](related-data.md)

Jednak czasami jednostki są wyszukiwane przy użyciu jednego wystąpienia kontekstu, a następnie zapisywane przy użyciu innego wystąpienia. Dzieje się tak często w scenariuszach "rozłączonych", takich jak aplikacja sieci web, w której jednostki są poszukiwane, wysyłane do klienta, modyfikowane, wysyłane z powrotem do serwera w żądaniu, a następnie zapisywane. W takim przypadku drugie wystąpienie kontekstu musi wiedzieć, czy jednostki są nowe (powinny być wstawiane) lub istniejące (powinny być aktualizowane).

<!-- markdownlint-disable MD028 -->
> [!TIP]
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) przykład artykułu na GitHub.

> [!TIP]
> EF Core może śledzić tylko jedno wystąpienie dowolnej jednostki o danej wartości klucza podstawowego. Najlepszym sposobem uniknięcia tego problemu jest użycie kontekstu krótkotrwałego dla każdej jednostki pracy, tak aby kontekst zaczyna się pusty, ma jednostki dołączone do niego, zapisuje te jednostki, a następnie kontekst jest usuwany i odrzucany.
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a>Identyfikowanie nowych jednostek

### <a name="client-identifies-new-entities"></a>Klient identyfikuje nowe jednostki

Najprostszym przypadkiem do czynienia jest, gdy klient informuje serwer, czy jednostka jest nowy lub istniejących. Na przykład często żądanie wstawienia nowej jednostki różni się od żądania aktualizacji istniejącej jednostki.

Pozostała część tej sekcji obejmuje przypadki, w których konieczne jest określenie w inny sposób, czy wstawić lub zaktualizować.

### <a name="with-auto-generated-keys"></a>Z automatycznie generowanymi klawiszami

Wartość automatycznie wygenerowanego klucza może być często używana do określenia, czy jednostka musi zostać wstawiona lub zaktualizowana. Jeśli klucz nie został ustawiony (oznacza to, że nadal ma domyślną wartość CLR null, zero itp.), jednostka musi być nowa i wymaga wstawienia. Z drugiej strony, jeśli wartość klucza została ustawiona, musi ona być już wcześniej zapisana, a teraz wymaga aktualizacji. Innymi słowy, jeśli klucz ma wartość, a następnie jednostka została zapytana, wysłane do klienta, a teraz wrócić do aktualizacji.

Łatwo jest sprawdzić, czy nie ustawiono klucza, gdy typ jednostki jest znany:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Jednak EF ma również wbudowany sposób, aby to zrobić dla dowolnego typu jednostki i typu klucza:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klucze są ustawiane tak szybko, jak jednostki są śledzone przez kontekst, nawet jeśli jednostka jest w stanie Dodane. Pomaga to podczas przechodzenia przez wykres jednostek i podejmowania decyzji, co zrobić z każdym, takich jak podczas korzystania z interfejsu API TrackGraph. Wartość klucza powinna być używana tylko w sposób pokazany w tym miejscu _przed_ każdym wywołaniem do śledzenia jednostki.

### <a name="with-other-keys"></a>Z innymi klawiszami

Jakiś inny mechanizm jest potrzebny do identyfikowania nowych jednostek, gdy wartości klucza nie są generowane automatycznie. Istnieją dwa ogólne podejścia do tego:

* Kwerenda dla encji
* Przekazywanie flagi od klienta

Aby zbadać encję, wystarczy użyć metody Znajdź:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Jest poza zakresem tego dokumentu, aby wyświetlić pełny kod do przekazywania flagi od klienta. W aplikacji sieci web zwykle oznacza dokonywanie różnych żądań dla różnych akcji lub przekazywanie niektórych stanów w żądaniu, a następnie wyodrębnianie go w kontrolerze.

## <a name="saving-single-entities"></a>Zapisywanie pojedynczych encji

Jeśli wiadomo, czy potrzebne jest wstawianie lub aktualizowanie, można odpowiednio użyć funkcji Dodaj lub Aktualizuj:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Jeśli jednak jednostka używa automatycznie generowanych wartości klucza, w obu przypadkach można użyć metody Update:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Update Metoda zwykle oznacza jednostki do aktualizacji, a nie wstawiania. Jeśli jednak encja ma klucz generowany automatycznie i nie ustawiono żadnej wartości klucza, jednostka jest automatycznie oznaczana do wstawienia.

> [!TIP]  
> To zachowanie zostało wprowadzone w EF Core 2.0. W przypadku wcześniejszych wydań zawsze należy jawnie wybrać dodaj lub aktualizuj.

Jeśli encja nie używa automatycznie generowanych kluczy, aplikacja musi zdecydować, czy encja powinna zostać wstawiona, czy aktualizowana: Na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Kroki są następujące:

* Jeśli funkcja Znajdź zwraca wartość null, baza danych nie zawiera jeszcze bloga o tym identyfikatorze, więc nazywamy Dodaj oznacz go do wstawienia.
* Jeśli funkcja Znajdź zwraca encję, istnieje ona w bazie danych, a kontekst jest teraz śledzeniu istniejącej encji
  * Następnie używamy SetValues, aby ustawić wartości dla wszystkich właściwości tej jednostki na te, które pochodziły od klienta.
  * Wywołanie SetValues oznaczy encję, która ma zostać zaktualizowana w razie potrzeby.

> [!TIP]  
> SetValues oznaczy tylko jako zmodyfikowane właściwości, które mają inne wartości do tych w śledzonej jednostki. Oznacza to, że po wysłaniu aktualizacji zostaną zaktualizowane tylko te kolumny, które faktycznie uległy zmianie. (A jeśli nic się nie zmieniło, żadna aktualizacja nie zostanie wysłana w ogóle.)

## <a name="working-with-graphs"></a>Praca z wykresami

### <a name="identity-resolution"></a>Rozpoznawanie tożsamości

Jak wspomniano powyżej, EF Core można śledzić tylko jedno wystąpienie dowolnej jednostki z danej wartości klucza podstawowego. Podczas pracy z wykresami wykres powinien być idealnie utworzony w taki sposób, aby ten niezmienny był zachowywany, a kontekst powinien być używany tylko dla jednej jednostki pracy. Jeśli wykres zawiera duplikaty, konieczne będzie przetworzenie wykresu przed wysłaniem go do EF, aby skonsolidować wiele wystąpień w jeden. Może to nie być trywialne, gdy wystąpienia mają sprzeczne wartości i relacje, więc konsolidacji duplikatów należy wykonać tak szybko, jak to możliwe w potoku aplikacji, aby uniknąć rozwiązywania konfliktów.

### <a name="all-newall-existing-entities"></a>Wszystkie nowe/wszystkie istniejące jednostki

Przykładem pracy z wykresami jest wstawianie lub aktualizowanie bloga wraz z jego kolekcją skojarzonych wpisów. Jeśli wszystkie jednostki na wykresie powinny być wstawione lub wszystkie powinny być aktualizowane, proces jest taki sam, jak opisano powyżej dla pojedynczych jednostek. Na przykład wykres blogów i postów utworzonych w ten sposób:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

można wstawić w ten sposób:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

Wywołanie dodaj oznaczy bloga i wszystkie wpisy, które mają zostać wstawione.

Podobnie, jeśli wszystkie jednostki na wykresie muszą zostać zaktualizowane, można użyć aktualizacji:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog i wszystkie jego posty zostaną oznaczone jako zaktualizowane.

### <a name="mix-of-new-and-existing-entities"></a>Połączenie nowych i istniejących podmiotów

W przypadku automatycznie generowanych kluczy aktualizacja może być ponownie używana zarówno dla wstawień, jak i aktualizacji, nawet jeśli wykres zawiera kombinację jednostek wymagających wstawiania i tych, które wymagają aktualizacji:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizacja oznaczy dowolną encję na wykresie, blogu lub poście do wstawienia, jeśli nie ma ustawionej wartości klucza, podczas gdy wszystkie inne jednostki są oznaczone do aktualizacji.

Tak jak poprzednio, gdy nie używa się automatycznie generowanych kluczy, można użyć kwerendy i niektórych przetwarzania:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Obsługa usuwania

Usuwanie może być trudne do obsługi, ponieważ często brak jednostki oznacza, że powinny zostać usunięte. Jednym ze sposobów radzenia sobie z tym jest użycie "nietrwałych usuwania", tak aby jednostka była oznaczona jako usunięta, a nie faktycznie usuwana. Usuwa następnie staje się taka sama jak aktualizacje. Usuwanie nietrwałe można zaimplementować przy użyciu [filtrów zapytań](xref:core/querying/filters).

W przypadku usuwania true wspólnego wzorca jest użycie rozszerzenia wzorca kwerendy do wykonywania tego, co jest zasadniczo różnicy graf. Przykład:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>Wykres dresowy

Wewnętrznie, Dodaj, Dołącz i Aktualizuj użyj wykresu przechodzenie z określeniem dla każdej jednostki, czy powinien być oznaczony jako Dodane (do wstawiania), Zmodyfikowane (do aktualizacji), Bez zmian (nic nie robić) lub usunięte (do usunięcia). Ten mechanizm jest narażony za pośrednictwem interfejsu API trackgraph. Załóżmy na przykład, że gdy klient odsyła wykres jednostek ustawia niektóre flagi na każdej jednostce wskazując, jak należy obsługiwać. TrackGraph może następnie służyć do przetwarzania tej flagi:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

Flagi są wyświetlane tylko jako część jednostki dla uproszczenia przykładu. Zazwyczaj flagi będą częścią DTO lub innego stanu uwzględnionego w żądaniu.
