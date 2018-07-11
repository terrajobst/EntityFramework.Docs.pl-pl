---
title: Migracje Code First przy użyciu istniejącej bazy danych — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
caps.latest.revision: 3
ms.openlocfilehash: 77e139a29bb4708b00fc6198a57780ce75197252
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949106"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migracje Code First przy użyciu istniejącej bazy danych
> [!NOTE]
> **EF4.3 począwszy tylko** — funkcje, interfejsów API itp. z opisem na tej stronie zostały wprowadzone w programie Entity Framework 4.1. Jeśli używasz starszej wersji, niektóre lub wszystkie informacje, nie ma zastosowania.

W tym artykule opisano, migracje Code First przy użyciu istniejącej bazy danych, który nie został utworzony przez program Entity Framework.

> [!NOTE]
> W tym artykule przyjęto założenie, że wiesz, jak użyć migracje Code First w podstawowych scenariuszy. Jeśli tego nie zrobisz, a następnie należy odczytać [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) przed kontynuowaniem.

## <a name="screencasts"></a>Zrzuty ekranu

Jeśli zrzut ekranu niż przeczytaj ten artykuł będzie wolisz obejrzeć, następujące dwa filmy wideo obejmuje tę samą zawartość, jak w tym artykule.

### <a name="video-one-migrations---under-the-hood"></a>Wideo 1: "Migracje - Under the Hood"

[Ten zrzut ekranu](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) opisano sposób migracji śledzi i używa informacji o modelu, aby wykrywać zmiany modelu.

### <a name="video-two-migrations---existing-databases"></a>Dwa wideo: "Migracje - istniejących baz danych"

Opierając się na koncepcji z poprzednim filmu wideo, [ten zrzut ekranu](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) opisano, jak włączyć i użyć migracje z istniejącej bazy danych.

## <a name="step-1-create-a-model"></a>Krok 1: Tworzenie modelu

Pierwszym krokiem będzie utworzenie model Code First, który jest przeznaczony dla istniejącej bazy danych. [Code First istniejącą bazę danych](~/ef6/modeling/code-first/workflows/existing-database.md) temat zawiera szczegółowe wskazówki, jak to zrobić.

>[!NOTE]
> Należy wykonać pozostałe kroki w tym temacie przed wprowadzeniem jakichkolwiek zmian, które wymagałyby zmiany schematu bazy danych modelu. Poniższe kroki wymagają modelu, który ma zostać zsynchronizowana ze schematem bazy danych.

## <a name="step-2-enable-migrations"></a>Krok 2: Włączanie funkcji migracje

Następnym krokiem jest włączanie funkcji migracje. Można to zrobić, uruchamiając **Enable-Migrations** polecenia w konsoli Menedżera pakietów.

To polecenie Utwórz folder w rozwiązaniu o nazwie migracje i umieścić jedną klasę wewnątrz niego nazywana konfiguracją. Klasa konfiguracji jest konfigurowania migracji dla aplikacji, możesz dowiedzieć się więcej na temat [migracje Code First](~/ef6/modeling/code-first/migrations/index.md) tematu.

## <a name="step-3-add-an-initial-migration"></a>Krok 3. Dodawanie początkowej migracji

Po migracji zostały utworzone i zastosować do lokalnej bazy danych, można także zastosować je zmieni się z innymi bazami danych. Na przykład lokalna baza danych może być test bazy danych i ostatecznie może chcesz również zastosować zmiany do produkcyjnej bazy danych i/lub innym deweloperom testowanie bazy danych. Dostępne są dwie opcje dla tego kroku i zależy od tego, który należy wybrać, czy schematów innych baz danych jest pusta lub obecnie jest zgodny ze schematem lokalnej bazy danych.

-   **Opcja jeden: Użyj istniejącego schematu jako punkt początkowy.** To podejście należy używać, gdy innych baz danych, które migracje będą dotyczyć w przyszłości będzie miał ten sam schemat, jak lokalna baza danych ma obecnie. Na przykład można użyć, to gdy bazy danych lokalnego testu jest obecnie zgodna v1 produkcyjnej bazy danych, a później będą stosowane te migracji, aby zaktualizować produkcyjnej bazy danych do wersji 2.
-   **Opcja 2: Użyj pustej bazy danych jako punkt początkowy.** To podejście należy używać, gdy innych baz danych, które migracje będą dotyczyć w przyszłości są puste (lub jeszcze nie istnieją). Na przykład można użyć, to gdy Rozpocznij tworzenie aplikacji swoją aplikację przy użyciu bazy danych testu, ale bez użycia migracje i można będzie później chcesz utworzyć produkcyjnej bazy danych od podstaw.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Opcja jeden: Użyj istniejącego schematu jako punktu wyjścia

Migracje Code First używa migawkę modelu przechowywane w najnowszych migracji do wykrywania zmian do modelu (można znaleźć szczegółowe informacje o tym w [migracje Code First na środowiska zespołowe](~/ef6/modeling/code-first/migrations/teams.md)). Ponieważ firma Microsoft zamierza przyjęto założenie, że bazy danych istnieje już schemat bieżącego modelu, wygenerujemy pusty migracji (pusta), które jest bieżący model jako migawka.

1.  Uruchom **InitialCreate migracji Dodaj — IgnoreChanges** polecenia w konsoli Menedżera pakietów. Spowoduje to utworzenie pustego migracji w obecnym modelu jako migawka.
2.  Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów. Migracja InitialCreate zostaną zastosowane do bazy danych. Ponieważ rzeczywistą migrację nie zawiera żadnych zmian, po prostu spowoduje dodanie wiersza do \_ \_MigrationsHistory tabeli wskazujący, że migracja została już zainstalowana.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Opcja 2: Użyj pustej bazy danych jako punktu wyjścia

W tym scenariuszu należy migracji, aby można było utworzyć całą bazę danych od podstaw — łącznie z tabelami, które już znajdują się w naszym lokalnej bazy danych. Zamierzamy Wygeneruj migrację InitialCreate, który zawiera logikę w celu utworzenia istniejącego schematu. Udoskonalimy naszej istniejącej bazy danych, wyglądają tak, ta migracja została już zainstalowana.

1.  Uruchom **InitialCreate migracji Dodaj** polecenia w konsoli Menedżera pakietów. Spowoduje to utworzenie migracji, aby utworzyć istniejącego schematu.
2.  Komentarz cały kod w metodzie w górę nowo utworzony migracji. Pozwoli to nam na element "apply" migracji w lokalnej bazie danych bez próby odtworzyć wszystkie tabele itp., które już istnieją.
3.  Uruchom **Update-Database** polecenia w konsoli Menedżera pakietów. Migracja InitialCreate zostaną zastosowane do bazy danych. Ponieważ nie zawiera rzeczywistą migrację zmiany (ponieważ możemy tymczasowo komentarzem nalepek), po prostu spowoduje dodanie wiersza do \_ \_MigrationsHistory tabeli wskazujący, że migracja została już zainstalowana.
4.  ONZ komentarz kod w metodzie w górę. Oznacza to, że podczas tej migracji jest stosowany do przyszłych baz danych, schemat, który istniał już w lokalnej bazie danych zostanie utworzona przez migracje.

## <a name="things-to-be-aware-of"></a>Co należy wiedzieć o

Istnieje kilka rzeczy, które musisz wiedzieć, korzystając z migracji z istniejącej bazy danych.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Domyślne/obliczane nazwy nie może odpowiadać istniejącego schematu

Migracje jawnie określa nazwy kolumn i tabel, gdy jej scaffolds migracji. Istnieją jednak inne migracje oblicza domyślnej nazwy dla podczas stosowania migracje obiekty bazy danych. Obejmuje to indeksy i ograniczenia klucza obcego. Podczas określania wartości schematu, te nazwy obliczeniowe może nie odpowiadać, co faktycznie istnieje w bazie danych.

Gdy muszą być tego świadomym, poniżej przedstawiono kilka przykładów:

**Jeśli użyto "jedną z opcji: Użyj istniejącego schematu jako punktu wyjścia" uzyskaną w kroku 3:**

-   Przyszłe zmiany w modelu wymagają, zmienianie lub porzucanie jeden z obiektów bazy danych, które są nazwane w różny sposób, należy zmodyfikować szkieletu migracji, aby określić poprawną nazwę. Interfejsy API migracji ma opcjonalny parametr nazwę, która pozwala na to.
    Na przykład istniejącego schematu może zawierać tabelę wpis z BlogId kolumny klucza obcego, która ma indeks o nazwie IndexFk\_BlogId. Jednak domyślnie migracje oczekiwać tego indeksu, aby mieć nazwę IX\_BlogId. Jeśli wprowadzisz zmiany w modelu, które powoduje usunięcie tego indeksu będzie konieczne zmodyfikowanie szkieletu wywołana DropIndex w celu określenia IndexFk\_BlogId nazwy.

**Jeśli użyto "dwóch opcji: Użyj pustej bazy danych jako punktu wyjścia" uzyskaną w kroku 3:**

-   Próby uruchomienia metody szczegółów początkowej migracji (które powracanie do pustej bazy danych) względem lokalnej bazy danych może zakończyć się niepowodzeniem, ponieważ migracje podejmie próbę Porzuć indeksy i ograniczenia klucza obcego przy użyciu niepoprawnymi nazwami. Wpłynie to tylko lokalna baza danych od innych baz danych zostanie utworzona od podstaw przy użyciu metody w górę początkowej migracji.
    Jeśli chcesz użyć istniejącej lokalnej bazy danych do stanu pustego najłatwiej można to zrobić ręcznie, poprzez usunięcie bazy danych lub usunięcie wszystkich tabel. Po tej początkowej obniżenia poziomu których wszystkie obiekty bazy danych zostaną odtworzone z domyślnymi nazwami więc ten problem nie przedstawi się ponownie.
-   Jeśli przyszłe zmiany w modelu wymagają, zmienianie lub porzucanie jeden z obiektów bazy danych, które są nazwane w różny sposób, to nie będzie działać względem istniejącej lokalnej bazy danych — od nazwy nie pasują do wartości domyślnych. Jednak będą działać względem bazy danych, które zostały utworzone "od zera", ponieważ te będą używane domyślne nazwy wybranego przez migracje.
    Można ręcznie wprowadzić te zmiany w istniejącej lokalnej bazy danych lub należy rozważyć utworzenie migracje ponownie utworzyć od podstaw — bazy danych, jak wpłynie to na innych komputerach.
-   Bazy danych utworzone przy użyciu metody w górę początkowej migracji mogą się nieznacznie różnić z lokalnej bazy danych od czasu obliczeniowego domyślne nazwy dla indeksów i ograniczeń klucza obcego, który będzie używany. Użytkownik może też pozostać przy użyciu dodatkowych indeksów jak migracji utworzy indeksy na kolumny klucza obcego domyślnie — to nie mogły być w przypadku oryginalnej bazy danych lokalnych.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Nie wszystkie obiekty bazy danych są reprezentowane w modelu

Obiekty bazy danych, które nie są częścią modelu nie będzie obsługiwany przez migracje. Może to obejmować widoki, procedury składowane, uprawnienia, tabel, które nie są częścią Twojego modelu, dodatkowe indeksy itp.

Gdy muszą być tego świadomym, poniżej przedstawiono kilka przykładów:

-   Niezależnie od opcji wybranej w kroku 3, jeśli wymagają przyszłe zmiany w modelu, zmienianie lub porzucanie tych obiektów dodatkowych migracji nie będzie wiedzieć, aby wprowadzić te zmiany. Na przykład jeśli usuniesz kolumny, która ma inny indeks, migracje będą nie wiedzieć, aby usunąć indeksu. Należy ręcznie dodać to do szkieletu migracji.
-   Jeśli użyto "dwóch opcji: Użyj pustej bazy danych jako punkt początkowy", te dodatkowe obiekty nie zostaną utworzone przez metodę w górę początkowej migracji.
    Można zmodyfikować górę i w dół do metody, aby zadbać o tych obiektów dodatkowych, jeśli chcesz. Dla obiektów, które nie są obsługiwane natywnie w interfejsie API migracje — takich jak widoki — możesz użyć [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) metodę, aby uruchomić pierwotne SQL do tworzenia i upuść je.
