---
title: Odłączone jednostki — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: a81b0a26fe98dcc1ddedc11aba2673338c8991e8
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948982"
---
# <a name="disconnected-entities"></a>Odłączone jednostki

Wystąpienie typu DbContext będzie automatycznie śledzić obiektów zwracanych z bazy danych. Zmiany wprowadzone do tych jednostek następnie zostanie wykryty, gdy SaveChanges nosi nazwę bazy danych zostaną zaktualizowane zgodnie z potrzebami. Zobacz [podstawowe Zapisz](basic.md) i [powiązanych danych](related-data.md) Aby uzyskać szczegółowe informacje.

Jednak czasami jednostki są wysyłane zapytanie przy użyciu jednego wystąpienia kontekstu i następnie zapisywane przy użyciu innego wystąpienia. To często zdarza się w scenariuszach "odłączonego", takich jak aplikacja sieci web, gdzie jednostki są badane, wysłane do klienta, modyfikować, wysyłanych z powrotem do serwera w żądaniu i następnie zapisany. W tym przypadku kontekstu drugiego wystąpienia musi wiedzieć, czy jednostki są nowe (powinien zostać wstawiony) lub istniejące (powinien zostać zaktualizowany).

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) w witrynie GitHub.

> [!TIP]
> EF Core można śledzić tylko jedno wystąpienie jednostki z danej wartości klucza podstawowego. Najlepszym sposobem, aby uniknąć tego problemu jest do użycia krótkotrwałe kontekstu dla każdej jednostki pracy w taki sposób, że kontekst zaczyna się puste, zawiera jednostki podłączone do niego, zapisuje te jednostki oraz kontekst jest usunięty i odrzucone.

## <a name="identifying-new-entities"></a>Identyfikowanie nowych jednostek

### <a name="client-identifies-new-entities"></a>Klient identyfikuje nowe jednostki

Najprostszym przypadku radzenia sobie z jest, gdy klient informuje serwer, czy jednostka jest nowym lub istniejącym. Na przykład często żądania, aby wstawić nowy obiekt różni się od żądanie zaktualizowania istniejącej jednostki.

Dalszej części tej sekcji omówiono przypadki gdzie go, które są niezbędne określić w inny sposób, czy należy wstawić lub zaktualizować.

### <a name="with-auto-generated-keys"></a>Za pomocą automatycznego generowania kluczy

Wartość automatycznie generowanego klucza często może służyć do określenia, czy jednostka musi być wstawiane lub aktualizowane. Jeśli nie ustawiono klucza (oznacza to, że nadal ma wartość domyślną CLR o wartości null, wartość zero, itp.), a jednostka musi być nowe musi wstawiania. Z drugiej strony Jeśli ustawiono wartość klucza, następnie go musi już zostały wcześniej zapisane i teraz wymaga aktualizacji. Innymi słowy Jeśli klucz ma wartość, a następnie jednostki badano wysłane do klienta i ma teraz wróć do zaktualizowania.

Jest łatwy do sprawdzenia nie ustawiono klucza, gdy znana jest typem jednostki:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

EF ma również wbudowane sposobem wykonania tego typu jednostki i typ klucza:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klucze są ustawione, tak szybko, jak jednostki są śledzone od kontekstu, nawet jeśli jednostka jest w stanie dodany. Dzięki temu podczas przechodzenia między wykres jednostek i podejmowania decyzji o postępowaniu z każdego, na przykład, jak za pomocą interfejsu API TrackGraph. Wartość klucza powinna służyć wyłącznie w taki sposób, w tym miejscu pokazano _przed_ wszelkie rozmowy do śledzenia jednostki.

### <a name="with-other-keys"></a>Przy użyciu innych kluczy

Niektóre inny mechanizm jest potrzebny do identyfikowania nowych jednostek, gdy wartości klucza nie są generowane automatycznie. Istnieją dwa ogólne podejścia do tego:
 * Zapytanie dla jednostki
 * Przekazać flagę od klienta

Aby wysłać zapytanie do jednostki, po prostu użyj metody Find:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Jest poza zakres tego dokumentu, aby wyświetlić pełny kod do przekazywania flagę od klienta. W aplikacji sieci web zwykle oznacza to, co innych żądań dla rozmaitych akcji lub przekazywanie pewnego stanu w żądaniu, a następnie wyodrębniania go w kontrolerze.

## <a name="saving-single-entities"></a>Zapisywanie pojedynczych jednostek

Jeśli wiadomo, czy jest potrzebny insert nebo update, a następnie dodaj lub zaktualizuj można odpowiednio:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Jednak jeśli jednostki używa automatycznego generowania wartości klucza, następnie metoda aktualizacji może służyć w obu przypadkach:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metoda aktualizacji zazwyczaj oznacza jednostkę do aktualizacji, wstawiania nie. Jednakże jeśli jednostka ma klucz wygenerowany automatycznie, a nie wartość klucza została ustawiona, a następnie jednostki zamiast tego jest automatycznie oznaczony do wstawienia.

> [!TIP]  
> To zachowanie została wprowadzona w programie EF Core 2.0. Dla wcześniejszych wersji należy zawsze jawnie wybrać Dodawanie lub aktualizowanie.

Jeśli jednostka nie korzysta z automatycznego generowania kluczy, a następnie aplikacja musi zdecydować, czy wstawione lub zaktualizowane jednostki: na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Dostępne są następujące kroki:
* Jeśli dodasz Znajdź zwraca wartość null, a następnie baza danych nie zawiera jeszcze blog o tym identyfikatorze, dlatego nazywamy Oznacz ją do wstawienia.
* Jeśli wyszukiwanie zwraca jednostkę, następnie istnieje w bazie danych i kontekstu jest teraz śledzenie istniejącej jednostki
  * Następnie używamy SetValues można ustawić wartości dla wszystkich właściwości dla tej jednostki do tych, które pochodzą od klienta.
  * Wywołanie SetValues spowoduje oznaczenie jednostkę którą chcesz zaktualizować, zgodnie z potrzebami.

> [!TIP]  
> SetValues oznaczy tylko zmienione właściwości, które mają różne wartości do tych w jednostce śledzone. Oznacza to, że gdy aktualizacja jest wysyłana, tylko te kolumny, które rzeczywiście zostały zmienione zostaną zaktualizowane. (I jeśli nic się nie zmieniło, aktualizacja nie zostanie wysłana w ogóle)

## <a name="working-with-graphs"></a>Praca z wykresami

### <a name="identity-resolution"></a>Rozwiązanie tożsamości

Jak wspomniano powyżej, programem EF Core tylko śledzić jednego wystąpienia z danej wartości klucza podstawowego jednostki. Pracując z wykresami wykres najlepiej powinny być tworzone, tak, aby ta niezmiennej jest utrzymywany i kontekst powinien być używany dla tylko jednej jednostki pracy. Jeśli wykres zawiera duplikaty, następnie będzie konieczne do przetworzenia na wykresie przed wysłaniem ich do programu EF w celu skonsolidowania wielu wystąpień w jednym. To może nie być prosta których wystąpienia mają wartościami będącymi w konflikcie i relacje, dlatego konsolidację duplikaty powinno się odbywać szybko, jak to możliwe w potoku aplikacji, aby uniknąć konfliktów.

### <a name="all-newall-existing-entities"></a>Wszystkie nowe/wszystkich istniejących jednostek.

Przykładem korzystania z wykresów jest wstawianie lub aktualizowania blogu wraz z jego kolekcja skojarzone wpisów. Powinien zostać wstawiony wszystkich jednostek w wykresie lub wszystkie powinny być aktualizowane, następnie proces jest taka sama, jak opisano powyżej dla jednej jednostki. Na przykład wykres blogów i wpisów umieszczonych w następujący sposób:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

mogą być wstawiane w następujący sposób:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Spowoduje to oznaczenie blogu i wszystkie wpisy, które ma zostać wstawiony wywołania do Add.

Podobnie jeśli wszystkie jednostki w grafie muszą zostać zaktualizowane, wówczas aktualizacji mogą być używane:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog i wszystkie jego wpisy zostaną oznaczone do zaktualizowania.

### <a name="mix-of-new-and-existing-entities"></a>Kombinacja nowych i istniejących jednostek

Za pomocą automatycznego generowania kluczy aktualizacja ponownie można zarówno dla operacji wstawienia i aktualizacje, nawet jeśli wykres zawiera różne jednostki, które wymagają, wstawianie i tych, które wymagają zaktualizowania:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizacja spowoduje oznaczenie dowolnej jednostki w wykresu, blogu lub wpis do wstawienia nie zainstalowano zestawu wartości klucza, podczas gdy inne jednostki są oznaczone do aktualizacji.

Jak wcześniej, bez korzystania z automatycznego generowania kluczy, zapytanie i jakieś operacje przetwarzania może być użyty:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Obsługa usuwa

Delete mogą być trudne do obsługi, ponieważ często Brak jednostki oznacza, że go usunąć. Jednym ze sposobów, aby poradzić sobie z tym jest używać "usuwania nietrwałego" w taki sposób, że jednostka jest oznaczony jako usunięty zamiast rzeczywistości usuwane. Usuwa, a następnie staje się taka sama jak aktualizacje. Usuwanie nietrwałego może być implementowany w przy użyciu [zapytania filtry](xref:core/querying/filters).

Usuwa wartość true, aby uzyskać wspólny wzorzec jest użycie rozszerzenia wzorca zapytania do wykonania, co to jest zasadniczo różnica wykresu Na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Wewnętrznie, Dodaj, Dołącz i aktualizacji za pomocą przechodzenie grafu przy podejmowaniu wprowadzone dla każdej jednostki do tego, czy jego powinien być oznaczony jako dodano (Aby wstawić), zmodyfikowany (w celu aktualizacji), Unchanged (NIC), lub usunięte (Aby usunąć). Ten mechanizm jest uwidaczniany za pomocą interfejsu API TrackGraph. Na przykład załóżmy, że gdy klient wysyła z powrotem wykres jednostek ustawia niektóre flagi dla każdej jednostki wskazujący sposób obsługi. TrackGraph następnie może służyć do przetwarzania tej flagi:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Flagi są wyświetlane tylko w ramach jednostki dla uproszczenia w przykładzie. Zazwyczaj flagi powinien być częścią obiekt DTO lub inny stan, zawarty w żądaniu.
