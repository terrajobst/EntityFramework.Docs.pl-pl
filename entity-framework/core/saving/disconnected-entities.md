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
# <a name="disconnected-entities"></a>Rozłączone jednostki

Wystąpienie DbContext automatycznie śledzi jednostki zwrócone z bazy danych. Zmiany wprowadzone w tych jednostkach zostaną następnie wykryte, gdy zostanie wywołana metody SaveChanges i baza danych zostanie zaktualizowana w razie potrzeby. Szczegóły można znaleźć w temacie Podstawowe dane dotyczące [zapisywania](basic.md) i [pokrewnych danych](related-data.md) .

Jednak czasami do jednostek są wysyłane zapytania przy użyciu jednego wystąpienia kontekstu, a następnie zapisane przy użyciu innego wystąpienia. Często zdarza się to w scenariuszach "rozłączonych", takich jak aplikacja sieci Web, do której są wysyłane zapytania, wysyłany do klienta, modyfikowane, wysyłane z powrotem do serwera w żądaniu, a następnie zapisane. W takim przypadku drugie wystąpienie kontekstu musi wiedzieć, czy jednostki są nowe (powinny być wstawiane) czy istniejące (należy je zaktualizować).

<!-- markdownlint-disable MD028 -->
> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) tego artykułu można wyświetlić w witrynie GitHub.

> [!TIP]
> EF Core może śledzić tylko jedno wystąpienie dowolnej jednostki z daną wartością klucza podstawowego. Najlepszym sposobem na uniknięcie tego problemu jest użycie kontekstu krótkotrwałego dla każdej jednostki pracy w taki sposób, że kontekst zaczyna puste, ma dołączone jednostki, zapisuje te jednostki, a następnie kontekst zostaje usunięty i odrzucony.
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a>Identyfikowanie nowych jednostek

### <a name="client-identifies-new-entities"></a>Klient identyfikuje nowe jednostki

Najprostszym przypadkiem, w którym należy się zająć, jest to, kiedy klient informuje serwer o tym, czy jednostka jest nowa czy istniejąca. Na przykład często żądanie wstawienia nowej jednostki różni się od żądania zaktualizowania istniejącej jednostki.

Pozostała część tej sekcji obejmuje przypadki, w których konieczne jest określenie w inny sposób, czy należy wstawić czy zaktualizować.

### <a name="with-auto-generated-keys"></a>Z kluczami generowanymi automatycznie

Wartość automatycznie generowanego klucza może być często używana do określenia, czy należy wstawić lub zaktualizować jednostkę. Jeśli klucz nie został ustawiony (oznacza to, że nadal ma wartość domyślną CLR o wartości null, zero itp.), jednostka musi być nowa i musi wstawiać. Z drugiej strony, jeśli wartość klucza została ustawiona, należy ją wcześniej zapisać i teraz wymaga aktualizacji. Innymi słowy, jeśli klucz ma wartość, jednostka została zbadana, wysłana do klienta i teraz ponownie zostanie zaktualizowana.

Jeśli wiadomo, że typ jednostki jest znany:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Jednakże EF ma wbudowaną metodę, aby to zrobić dla dowolnego typu jednostki i typu klucza:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klucze są ustawiane, gdy tylko jednostki są śledzone przez kontekst, nawet jeśli jednostka jest w stanie dodany. Ułatwia to przechodzenie grafu jednostek i decydowanie o tym, co należy zrobić z każdym z nich, na przykład w przypadku korzystania z interfejsu API TrackGraph. Wartość klucza powinna być używana tylko w sposób przedstawiony tutaj _przed_ wywołaniem do śledzenia jednostki.

### <a name="with-other-keys"></a>Z innymi kluczami

Aby identyfikować nowe jednostki w przypadku, gdy wartości kluczy nie są generowane automatycznie, jest wymagany inny mechanizm. Istnieją dwa ogólne podejścia do tego:

* Zapytanie dotyczące jednostki
* Przekaż flagę z klienta

Aby wykonać zapytanie dotyczące jednostki, po prostu Użyj metody Find:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Ten dokument wykracza poza zakres tego dokumentu, aby pokazać pełen kod przekazywania flagi z klienta. W aplikacji sieci Web zazwyczaj oznacza to, że różne żądania dla różnych akcji lub przekazywanie pewnych stanów w żądaniu są wyodrębniane w kontrolerze.

## <a name="saving-single-entities"></a>Zapisywanie pojedynczych jednostek

Jeśli wiadomo, czy jest wymagana INSERT lub Update, można odpowiednio użyć opcji Dodaj lub zaktualizuj:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Jeśli jednak jednostka używa automatycznie generowanych wartości kluczy, metoda Update może być używana w obu przypadkach:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metoda Update zwykle oznacza jednostkę do aktualizacji, nie wstawiaj. Jeśli jednak jednostka ma automatycznie wygenerowany klucz i żadna wartość klucza nie została ustawiona, jednostka zostanie automatycznie oznaczona do wstawienia.

> [!TIP]  
> To zachowanie zostało wprowadzone w EF Core 2,0. W przypadku wcześniejszych wersji zawsze konieczne jest jawne wybranie opcji Dodaj lub zaktualizuj.

Jeśli jednostka nie używa automatycznie generowanych kluczy, aplikacja musi zdecydować, czy jednostka powinna zostać wstawiona lub zaktualizowana: na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Poniżej przedstawiono następujące czynności:

* Jeśli funkcja Znajdź zwraca wartość null, baza danych nie zawiera jeszcze blogu o tym IDENTYFIKATORze, więc wywołamy polecenie Dodaj markę do wstawienia.
* Jeśli funkcja Znajdź zwraca jednostkę, w bazie danych istnieje wartość, a w kontekście jest teraz śledzona Istniejąca jednostka
  * Następnie użyjemy setValues, aby ustawić wartości dla wszystkich właściwości tej jednostki dla tych, które zostały dostarczone przez klienta.
  * Wywołanie setValues oznaczy jednostkę, która ma zostać zaktualizowana w razie potrzeby.

> [!TIP]  
> Wartości setValues będą oznaczane jako zmodyfikowane właściwości, które mają różne wartości w monitorowanej jednostce. Oznacza to, że po wysłaniu aktualizacji zostaną zaktualizowane tylko te kolumny, które zostały zmienione w rzeczywistości. (A jeśli nic się nie zmieniło, aktualizacja nie zostanie wysłana wcale.)

## <a name="working-with-graphs"></a>Praca z wykresami

### <a name="identity-resolution"></a>Rozpoznawanie tożsamości

Jak wspomniano powyżej, EF Core może śledzić tylko jedno wystąpienie dowolnej jednostki z daną wartością klucza podstawowego. Podczas pracy z wykresami, wykres powinien być utworzony w taki sposób, że niezmienna jest utrzymywana, a kontekst powinien być używany tylko dla jednej jednostki pracy. Jeśli wykres zawiera duplikaty, konieczne będzie przetworzenie wykresu przed wysłaniem go do EF, aby skonsolidować wiele wystąpień w jeden. Może to nie być nieuproszczone miejsce, w którym wystąpienia mają wartości powodujące konflikt i relacje, dlatego konsolidacja duplikatów powinna być wykonywana najszybciej, jak to możliwe w potoku aplikacji, aby uniknąć rozwiązywania konfliktów.

### <a name="all-newall-existing-entities"></a>Wszystkie nowe/wszystkie istniejące jednostki

Przykładem pracy z wykresami jest wstawianie lub aktualizowanie blogu wraz z kolekcją skojarzonych wpisów. Jeśli wszystkie jednostki na grafie powinny zostać wstawione lub wszystkie powinny zostać zaktualizowane, proces jest taki sam jak w powyższym obszarze dla pojedynczych jednostek. Na przykład Graf blogów i wpisów utworzonych w następujący sposób:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

można go wstawić w następujący sposób:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

Wywołanie do dodania spowoduje oznaczenie blogu i wszystkich wpisów do wstawienia.

Podobnie, jeśli wszystkie jednostki na grafie muszą zostać zaktualizowane, można użyć aktualizacji:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog i wszystkie jego wpisy zostaną oznaczone jako do zaktualizowania.

### <a name="mix-of-new-and-existing-entities"></a>Mieszanie nowych i istniejących jednostek

Dzięki automatycznie generowanym kluczom aktualizacja może być ponownie używana dla operacji INSERT i Update, nawet jeśli wykres zawiera różne jednostki, które wymagają wstawiania i te, które wymagają aktualizacji:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizacja spowoduje oznaczenie dowolnej jednostki w grafie, blogu lub wpisie do wstawienia, jeśli nie ma ustawionej wartości klucza, a wszystkie inne jednostki są oznaczone do aktualizacji.

Tak jak wcześniej, gdy nie korzystasz z kluczy generowanych automatycznie, można użyć zapytania i niektórych operacji przetwarzania:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Obsługa usunięć

Usuwanie może być trudne do obsłużenia, ponieważ często nie istnieje nieobecność jednostki, ponieważ należy ją usunąć. Jednym ze sposobów postępowania z tym jest użycie "Usuwanie miękkie" w taki sposób, że jednostka jest oznaczona jako usunięta, a nie jest faktycznie usuwana. Następnie usuwane są takie same jak aktualizacje. Usuwanie miękkie można zaimplementować przy użyciu [filtrów zapytań](xref:core/querying/filters).

W przypadku usunięć "true" typowym wzorcem jest użycie rozszerzenia wzorca zapytania, aby wykonać to, co jest zasadniczo różnicą wykresu. Na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Wewnętrznie, dodawać, dołączać i aktualizować użycie programu Graph-przechodzenie przy użyciu oznaczeń wykonanych dla każdej jednostki w taki sposób, że powinna zostać oznaczona jako dodana (do wstawienia), zmodyfikowana (do aktualizacji), niezmieniona (nic nie dotyczy) lub usunięta (do usunięcia). Ten mechanizm jest udostępniany za pośrednictwem interfejsu API TrackGraph. Załóżmy na przykład, że gdy klient wysyła wykres z tyłu jednostek, ustawia dla każdej jednostki flagę wskazującą, jak powinna być obsługiwana. TrackGraph można następnie użyć do przetworzenia tej flagi:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

Flagi są wyświetlane tylko jako część jednostki dla uproszczenia przykładu. Zwykle flagi będą częścią DTO lub innego stanu zawartego w żądaniu.
