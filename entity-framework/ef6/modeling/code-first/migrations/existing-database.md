---
title: Migracje Code First z istniejącą bazą danych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: eb7948eafb1322cabcf69b47bd5411f762fe8498
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418994"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migracje Code First z istniejącą bazą danych
> [!NOTE]
> **Ef 4.3 tylko** — funkcje, interfejsy API itp. omówione na tej stronie zostały wprowadzone w Entity Framework 4,1. Jeśli używasz wcześniejszej wersji, niektóre lub wszystkie informacje nie są stosowane.

W tym artykule omówiono używanie Migracje Code First z istniejącą bazą danych, która nie została utworzona przez Entity Framework.

> [!NOTE]
> W tym artykule założono, że wiesz, jak używać Migracje Code First w podstawowych scenariuszach. Jeśli tego nie zrobisz, musisz przeczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="screencasts"></a>Screencasts

Jeśli wolisz obejrzeć zrzut ekranu przedstawiający niż odczytanie tego artykułu, następujące dwa wideo obejmują tę samą zawartość co ten artykuł.

### <a name="video-one-migrations---under-the-hood"></a>Wideo one: "migracje — pod okapem"

[Ten zrzut ekranu przedstawiający](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) dotyczy sposobu, w jaki migracja śledzi i używa informacji o modelu do wykrywania zmian modelu.

### <a name="video-two-migrations---existing-databases"></a>Wideo dwa: "migracje — istniejące bazy danych"

W [tym zrzut ekranu przedstawiający](https://channel9.msdn.com/blogs/ef/migrations-existing-databases) omówiono sposób włączania i używania migracji z istniejącą bazą danych.

## <a name="step-1-create-a-model"></a>Krok 1. Tworzenie modelu

Pierwszym krokiem będzie utworzenie modelu Code First, który jest przeznaczony dla istniejącej bazy danych. [Code First do istniejącego tematu bazy danych](~/ef6/modeling/code-first/workflows/existing-database.md) zawiera szczegółowe wskazówki na temat tego, jak to zrobić.

>[!NOTE]
> Należy postępować zgodnie z pozostałymi krokami tego tematu przed wprowadzeniem jakichkolwiek zmian w modelu, które wymagają zmian w schemacie bazy danych. Poniższe kroki wymagają, aby model był zsynchronizowany ze schematem bazy danych.

## <a name="step-2-enable-migrations"></a>Krok 2. Włączanie migracji

Następnym krokiem jest włączenie migracji. Można to zrobić, uruchamiając polecenie **enable-migrations** w konsoli Menedżera pakietów.

To polecenie spowoduje utworzenie folderu w rozwiązaniu o nazwie migrations i umieszczenie w nim pojedynczej klasy o nazwie Konfiguracja. W klasie konfiguracji można skonfigurować migracje dla aplikacji, aby uzyskać więcej informacji na jej temat w temacie [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="step-3-add-an-initial-migration"></a>Krok 3. Dodawanie początkowej migracji

Po utworzeniu i zastosowaniu migracji do lokalnej bazy danych można również zastosować te zmiany do innych baz danych. Na przykład lokalna baza danych może być testową bazą danych i możesz ostatecznie chcieć zastosować zmiany do produkcyjnej bazy danych i/lub innych deweloperów testowych baz danych. Dostępne są dwie opcje tego kroku, a ten, który należy wybrać, zależy od tego, czy schemat innych baz danych jest pusty czy jest obecnie zgodny ze schematem lokalnej bazy danych.

-   **Opcja 1: Użyj istniejącego schematu jako punktu początkowego.** Należy użyć tej metody, gdy w przyszłości zostaną zastosowane inne bazy danych, do których migracja zostanie zastosowana, będzie mieć taki sam schemat jak lokalna baza danych. Na przykład możesz użyć tego ustawienia, jeśli lokalna baza danych testowa jest obecnie zgodna z wersją 1 w produkcyjnej bazie danych i później zostaną zastosowane te migracje w celu zaktualizowania produkcyjnej bazy danych do wersji 2.
-   **Opcja 2: Użyj pustej bazy danych jako punktu początkowego.** Należy użyć tej metody, gdy inne bazy danych, do których zostaną zastosowane migracje, są puste (lub jeszcze nie istnieją). Można to na przykład użyć, jeśli rozpoczęto tworzenie aplikacji przy użyciu testowej bazy danych, ale bez użycia migracji, a później utworzysz produkcyjną bazę danych od podstaw.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Opcja 1: Użyj istniejącego schematu jako punktu początkowego

Migracje Code First używa migawki modelu przechowywanego w najnowszej migracji w celu wykrywania zmian w modelu (szczegółowe informacje na ten temat można znaleźć w [migracje Code First w środowiskach zespołu](~/ef6/modeling/code-first/migrations/teams.md)). Ponieważ chcemy założyć, że bazy danych mają już schemat bieżącego modelu, firma Microsoft wygeneruje pustą migrację (No-op), która ma bieżący model jako migawkę.

1.  Uruchom polecenie **Add-Migration InitialCreate – IgnoreChanges** w konsoli Menedżera pakietów. Spowoduje to utworzenie pustej migracji z bieżącym modelem jako migawką.
2.  Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów. Spowoduje to zastosowanie migracji InitialCreate do bazy danych programu. Ponieważ rzeczywista migracja nie zawiera żadnych zmian, po prostu dodaje wiersz do \_\_tabeli MigrationsHistory wskazującej, że migracja została już zastosowana.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Opcja 2: Użyj pustej bazy danych jako punktu początkowego

W tym scenariuszu potrzebujemy migracji, aby można było utworzyć całą bazę danych od podstaw — w tym tabele, które znajdują się już w lokalnej bazie danych. Będziemy generować migrację InitialCreate, która obejmuje logikę do utworzenia istniejącego schematu. Następnie wprowadzimy istniejącą bazę danych, która będzie wyglądać tak, jak ta migracja została już zastosowana.

1.  Uruchom polecenie **Add-Migration InitialCreate** w konsoli Menedżera pakietów. Spowoduje to utworzenie migracji, aby utworzyć istniejący schemat.
2.  Skomentuj cały kod w metodzie nowo utworzonej migracji. Umożliwi to "zastosowanie" migracji do lokalnej bazy danych bez konieczności ponownego tworzenia wszystkich tabel itd.
3.  Uruchom polecenie **Update-Database** w konsoli Menedżera pakietów. Spowoduje to zastosowanie migracji InitialCreate do bazy danych programu. Ponieważ rzeczywista migracja nie zawiera żadnych zmian (ponieważ tymczasowo Komentarze zostały uwzględnione), po prostu doda wiersz do \_\_tabeli MigrationsHistory wskazującej, że migracja została już zastosowana.
4.  Odskomentuj kod w metodzie up. Oznacza to, że w przypadku zastosowania tej migracji do przyszłych baz danych schemat, który już istniał w lokalnej bazie danych, zostanie utworzony przez migracje.

## <a name="things-to-be-aware-of"></a>Zagadnienia, z którymi należy się zapoznać

Należy pamiętać o kilku kwestiach dotyczących migracji do istniejącej bazy danych.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Nazwy domyślne/obliczeniowe mogą być niezgodne z istniejącym schematem

Migracje jawnie określają nazwy kolumn i tabel podczas tworzenia szkieletu migracji. Istnieją jednak inne obiekty bazy danych, dla których migracja jest obliczana jako domyślna nazwa w przypadku stosowania migracji. Obejmuje to ograniczenia dotyczące indeksów i kluczy obcych. W przypadku określania wartości docelowej istniejący schemat te obliczone nazwy mogą nie odpowiadać tym, co rzeczywiście istnieje w bazie danych.

Poniżej przedstawiono kilka przykładów, które należy wziąć pod uwagę:

**Jeśli użyto opcji jeden: Użyj istniejącego schematu jako punktu początkowego od kroku 3:**

-   Jeśli przyszłe zmiany w modelu wymagają zmiany lub porzucenie jednego z obiektów bazy danych o nazwie inaczej, należy zmodyfikować migrację szkieletową, aby określić poprawną nazwę. Interfejsy API migracji mają opcjonalny parametr nazwy, który umożliwia wykonanie tej czynności.
    Na przykład istniejący schemat może mieć tabelę ogłoszeń z kolumną klucza obcego BlogId, która ma indeks o nazwie IndexFk\_BlogId. Jednak domyślnie migracje będą oczekiwać, że ten indeks ma nazwę IX\_BlogId. Jeśli wprowadzisz zmiany w modelu, który spowoduje porzucenie tego indeksu, musisz zmodyfikować wywołanie DropIndex szkieletowej, aby określić nazwę IndexFk\_BlogId.

**Jeśli użyto opcji "Option dwa: Użyj pustej bazy danych jako punktu początkowego" z kroku 3:**

-   Próba uruchomienia metody w dół migracji początkowej (czyli przywrócenie pustej bazy danych) do lokalnej bazy danych może się nie powieść, ponieważ migracja podejmie próbę porzucenia indeksów i ograniczeń klucza obcego przy użyciu niepoprawnych nazw. Będzie to miało wpływ tylko na lokalną bazę danych, ponieważ inne bazy danych zostaną utworzone od początku przy użyciu metody początkowej migracji.
    Jeśli chcesz obniżyć poziom istniejącej lokalnej bazy danych do pustego stanu, najłatwiej wykonać tę czynność ręcznie, usuwając bazę danych lub upuszczając wszystkie tabele. Po rozpoczęciu wstępnego obniżenia poziomu wszystkie obiekty bazy danych zostaną ponownie utworzone przy użyciu nazw domyślnych, więc ten problem nie będzie już występować.
-   Jeśli przyszłe zmiany w modelu wymagają zmiany lub porzucenie jednego z obiektów bazy danych o nazwie inaczej, nie będzie to możliwe w przypadku istniejącej lokalnej bazy danych — ponieważ nazwy nie będą zgodne z ustawieniami domyślnymi. Jednak będzie ona działała względem baz danych, które zostały utworzone "od podstaw", ponieważ będą używały domyślnych nazw wybranych przez migracje.
    Można wprowadzić te zmiany ręcznie w lokalnej istniejącej bazie danych lub rozważyć przeprowadzenie migracji od podstaw, tak jak w przypadku innych maszyn.
-   Bazy danych utworzone przy użyciu metody up migracji początkowej mogą się nieco różnić od lokalnej bazy danych, ponieważ obliczone domyślne nazwy indeksów i ograniczeń klucza obcego zostaną użyte. Możesz również zakończyć z dodatkowymi indeksami, ponieważ migracja domyślnie utworzy indeksy w kolumnach klucza obcego — może to nie być przypadek w pierwotnej lokalnej bazie danych.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Nie wszystkie obiekty bazy danych są reprezentowane w modelu

Obiekty bazy danych, które nie są częścią modelu, nie będą obsługiwane przez migracje. Może to obejmować widoki, procedury składowane, uprawnienia, tabele, które nie są częścią modelu, dodatkowe indeksy itd.

Poniżej przedstawiono kilka przykładów, które należy wziąć pod uwagę:

-   Bez względu na opcję wybraną w kroku 3, jeśli przyszłe zmiany w modelu wymagają zmiany lub porzucania tych dodatkowych obiektów migracja nie będzie wiadomo, że te zmiany zostaną wprowadzone. Na przykład jeśli porzucasz kolumnę z dodatkowym indeksem, migracja nie będzie wiadomo, aby usunąć indeks. Należy ręcznie dodać to do migracji szkieletowej.
-   Jeśli użyto opcji "Option dwa: Użyj pustej bazy danych jako punktu początkowego", te dodatkowe obiekty nie zostaną utworzone przy użyciu metody up początkowej migracji.
    Możesz zmodyfikować metody w górę i w dół, aby zadbać o te dodatkowe obiekty. W przypadku obiektów, które nie są natywnie obsługiwane w interfejsie API migracji — takich jak widoki — można użyć metody [SQL](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) do uruchomienia nieprzetworzonego SQL w celu utworzenia/porzucenia.
