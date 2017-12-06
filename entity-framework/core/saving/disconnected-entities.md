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
# <a name="disconnected-entities"></a>Odłączonych jednostek.

Wystąpienie typu DbContext automatycznie śledzi zwróconych z bazy danych. Zmiany wprowadzone do tych jednostek następnie zostanie wykryty, gdy jest wywoływana SaveChanges i bazy danych zostanie zaktualizowany zgodnie z potrzebami. Zobacz [podstawowe zapisać](basic.md) i [dane dotyczące](related-data.md) szczegółowe informacje.

Jednak czasami jednostek będą pytani, przy użyciu jednego wystąpienia kontekstu, a następnie zapisane przy użyciu innego wystąpienia. Odbywa się to często w "odłączony" scenariuszy, takich jak aplikacji sieci web, w którym jednostek są proszeni, wysyłane do klienta zmodyfikowane, wysyłane z powrotem do serwera w żądaniu i zapisany. W takim przypadku kontekst drugiego wystąpienia musi wiedzieć, czy jednostki są nowe (powinny zostać wstawione) lub istniejące (powinien być zaktualizowany).

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) w witrynie GitHub.

## <a name="identifying-new-entities"></a>Identyfikowanie nowych jednostek

### <a name="client-identifies-new-entities"></a>Klient identyfikuje nowe jednostki

Najprostszym przypadku radzenia sobie z jest, gdy klient informuje serwer, czy jednostka jest nowy lub istniejący. Na przykład często żądania, aby wstawić nową jednostkę różni się od żądania w celu zaktualizowania istniejącej jednostki.

W pozostałej części tej sekcji omówiono przypadkach gdy niezbędne do określenia w inny sposób, czy można wstawić ani zaktualizować go.

### <a name="with-auto-generated-keys"></a>Z automatycznego generowania kluczy

Wartość klucza automatycznie generowanych często może służyć do określenia, czy jednostka musi być wstawiane lub aktualizowane. Jeśli klucz nie został ustawiony, (tj. nadal posiada CLR domyślna wartość null, zero, itp.), jednostki musi być nowy i wymaga Wstawianie. Z drugiej strony Jeśli ustawiono wartość tego klucza, następnie je musi już zostały wcześniej zapisane i teraz konieczna jest aktualizacja. Innymi słowy Jeśli klucz ma wartość, następnie jednostki kwerendy, wysyłane do klienta i ma teraz wróć do zaktualizowania.

Jest łatwy do sprawdzenia nie ustawiono klucza, gdy jest znany typ jednostki:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

EF ma również wbudowane sposób w tym celu dla typu jednostki, a typ klucza:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Klucze są ustawiane jak jednostki są śledzone przez kontekst, nawet jeśli obiekt jest w stanie Added. Dzięki temu podczas przesyłania wykres jednostek i decydowanie o każdym, takich jak przy użyciu interfejsu API TrackGraph. Wartość klucza należy używać tylko w taki sposób, w tym miejscu pokazano _przed_ żadnych wywołanie do śledzenia jednostki.

### <a name="with-other-keys"></a>Z kluczy

Niektóre inny mechanizm jest potrzebna do nowych jednostek tożsamości, gdy wartości klucza nie są generowane automatycznie. Istnieją dwie metody ogólne do poniższego:
 * Zapytanie dla jednostki
 * Przekaż flagę z klienta

Dla obiekt kwerendy, po prostu użyj metody Find:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Wykracza poza zakres tego dokumentu, aby wyświetlić pełny kod może być przekazywany flagę od klienta. W aplikacji sieci web zwykle oznacza to, wysyłania żądań różne dla różnych działań, lub przekazywanie niektórych stanu w żądaniu, a następnie wyodrębnij go w kontrolerze.

## <a name="saving-single-entities"></a>Zapisywanie pojedynczych jednostek

Jeśli wiadomo, czy jest konieczne insert lub update, a następnie dodaj lub zaktualizuj można odpowiednio:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Jednak jeśli jednostka używa automatycznego generowania wartości klucza, następnie metody aktualizacji można użyć w obu przypadkach:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Metody aktualizacji zwykle oznacza jednostki aktualizacji nie insert. Jednak jeśli jednostka ma klucz wygenerowany automatycznie, a nie wartość klucza nie ustawiono, jednostki zamiast tego jest automatycznie oznaczone do wstawienia.

> [!TIP]  
> To zachowanie została wprowadzona w programie EF Core 2.0. W starszych wersjach należy zawsze jawnie wybierz polecenie Dodaj lub zaktualizuj.

Jeśli jednostka nie używa automatycznego generowania kluczy, a następnie aplikacji należy zdecydować, czy jednostka powinna być wstawiane lub aktualizowane: na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Czynności opisane w tym miejscu są:
* Dodaj Znajdź zwraca wartość null, a następnie bazy danych nie zawiera już blog o tym identyfikatorze, dlatego nazywamy Oznacz ją do wstawienia.
* Jeśli Znajdź zwraca jednostkę, następnie istnieje w bazie danych i kontekst teraz śledzi istniejącej jednostki
  * Następnie używamy SetValues można ustawić wartości dla wszystkich właściwości dla tej jednostki do tych, które pochodzą od klienta.
  * Wywołanie SetValues oznaczy jednostka do zaktualizowania zgodnie z potrzebami.

> [!TIP]  
> SetValues oznaczy tylko modyfikowanie właściwości, które mają różne wartości określone w śledzonych jednostki. Oznacza to, że po wysłaniu aktualizacji, tylko te kolumny, które faktycznie zostały zmienione zostaną zaktualizowane. (I nic się nie zmieniło, aktualizacja nie zostanie wysłana na wszystkich.)

## <a name="working-with-graphs"></a>Praca z wykresów

### <a name="all-newall-existing-entities"></a>Wszystkie nowe/wszystkich istniejących obiektów.

Przykład pracy wykresami jest wstawiania lub aktualizowania blogu wraz z jego kolekcja wpisów skojarzone. Jeśli wszystkie jednostki na wykresie powinny zostać wstawione lub wszystkie powinien zostać zaktualizowany, następnie proces jest taka sama, jak opisano powyżej dla jednej jednostki. Na przykład wykres blogów i wpisy utworzone w następujący sposób:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

mogą być wstawiane następująco:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Wywołania do Add oznaczy blogu i wszystkie wpisy do wstawienia.

Podobnie jeśli wszystkie jednostki na wykresie muszą zostać zaktualizowane, następnie aktualizacja można:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog i wszystkie jego wpisy zostaną oznaczone do zaktualizowania.

### <a name="mix-of-new-and-existing-entities"></a>Mieszane nowych i istniejących jednostek

Z automatycznego generowania kluczy aktualizacji ponownie można dla operacji wstawiania i aktualizacji, nawet jeśli wykres zawiera różnych jednostek, które wymagają Wstawianie oraz te, które wymagają zaktualizowania:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Aktualizacja spowoduje oznaczenie dowolnej jednostki na wykresie, blog lub post przez operację wstawiania, jeśli nie ma ustawioną wartość klucza, podczas gdy inne jednostki są oznaczone dla aktualizacji.

Jako przed, gdy nie używa automatycznego generowania kluczy, kwerendy i przetwarza mogą być używane:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Obsługa usuwa

Delete mogą być trudnych do obsługi, ponieważ często Brak jednostki oznacza, że należy ją usunąć. Jednym ze sposobów postępowania w przypadku to jest do użycia "usuwania nietrwałego" w taki sposób, że jednostka jest oznaczone jako usunięte zamiast faktycznie usunięte. Usuwa, a następnie staje się taka sama jak aktualizacje. Elastyczne usuwa można zaimplementować przy użyciu [zapytania filtry](xref:core/querying/filters).

Dla usunięć wartość true jest użycie rozszerzenia wzorca zapytania do wykonania, co to jest zasadniczo różn. wykres wspólnego wzorca Na przykład:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Wewnętrznie, Dodaj Attach i aktualizacji za pomocą wykresu przechodzenie określenie wprowadzone dla każdej jednostki określające, czy jego powinien być oznaczony jako Added (Aby wstawić), zmodyfikowany (w celu aktualizacji), Unchanged (nic nie rób), lub usunięte (Aby usunąć). Ten mechanizm jest uwidaczniany za pomocą interfejsu API TrackGraph. Na przykład załóżmy, że że gdy klient przesyła wykres jednostek ustawia niektóre flagi dla każdej jednostki wskazującą sposób obsługi. Następnie można TrackGraph do przetworzenia tej flagi:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Flagi są wyświetlane tylko w ramach jednostki dla uproszczenia przykładu. Zazwyczaj flagi powinien być częścią DTO lub inny stan zawarte w żądaniu.
