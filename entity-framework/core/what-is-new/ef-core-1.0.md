---
title: Co to jest nowa w programie EF 1.0 Core - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: e5b9e57a01ff302b1d7bd0fc5419aa5b8213865e
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="features-included-in-ef-core-10"></a>Funkcje uwzględnione w wersji 1.0 Core EF

## <a name="platforms"></a>Platformy
### <a name="net-framework-451"></a>.NET Framework 4.5.1
Obejmuje konsoli WPF, WinForms, platformy ASP.NET 4, itp.
### <a name="net-standard-13"></a>.NET standard 1.3
W tym platformy ASP.NET Core przeznaczonych dla zarówno .NET Framework i .NET Core systemu Windows, OS x i Linux.

## <a name="modelling"></a>Modelowanie
### <a name="basic-modelling"></a>Podstawowe modelowania
Oparte na jednostki POCO z pobierania/ustawiania właściwości popularnych typów skalarnych (`int`, `string`itp.).
### <a name="relationships-and-navigation-properties"></a>Relacje i właściwości nawigacji
Można określić jeden do wielu i relacje jeden do zero lub jeden w modelu, na podstawie klucza obcego. Właściwości nawigacji prostego typu Kolekcja lub odwołanie mogą być skojarzone z tych relacji.
### <a name="built-in-conventions"></a>Konwencje wbudowane
Te kompilacji początkowej model oparty na kształt klas jednostek.
### <a name="fluent-api"></a>Interfejsu API Fluent
Pozwala zastąpić `OnModelCreating` metody dla kontekstu można dodatkowo skonfigurować modelu, która została wykryta przez Konwencję.
### <a name="data-annotations"></a>Adnotacje danych
Są to atrybuty, które mogą zostać dodane do właściwości klasy jednostki i będzie mieć wpływ modelu EF (tj. dodając EF wiedzieć, że wymagana jest właściwość let będzie [wymagane]).
### <a name="relational-table-mapping"></a>Relacyjne mapowania tabeli
Umożliwia jednostek można mapować na tabele/kolumny.
### <a name="key-value-generation"></a>Generowanie wartości klucza
W tym generacji po stronie klienta i bazy danych.
### <a name="database-generated-values"></a>Baza danych wygenerowała wartości
Zezwala na wartości, które ma być generowany przez bazę danych przy wstawianiu (wartości domyślne) lub aktualizacji (kolumn obliczanych).
### <a name="sequences-in-sql-server"></a>Sekwencje w programie SQL Server
Zezwala na obiekty sekwencji ma zostać zdefiniowana w modelu.
### <a name="unique-constraints"></a>Ograniczenia unikalne
Umożliwia definicji klucze alternatywne i możliwość definiowania relacji obiektu docelowego tego klucza.
### <a name="indexes"></a>Indeksy
Definiowanie indeksów w modelu automatycznie wprowadza indeksy w bazie danych. Unikatowe indeksy są również obsługiwane.
### <a name="shadow-state-properties"></a>Właściwości stanu w tle
Umożliwia właściwości ma zostać zdefiniowana w modelu, które nie został zadeklarowany i nie są przechowywane w klasie .NET, ale można śledzić i zaktualizowane przez EF Core. Powszechnie używane dla właściwości klucza obcego, gdy udostępnianie w obiekt nie jest wymagana.
### <a name="table-per-hierarchy-inheritance-pattern"></a>Wzorzec dziedziczenia tabeli na hierarchii
Umożliwia jednostek w hierarchii dziedziczenia do zapisania pojedynczej tabeli, aby zidentyfikować ich typ jednostki dla danego rekordu w bazie danych przy użyciu kolumny rozróżniacza.
### <a name="model-validation"></a>Weryfikacja modelu
Wykrywa nieprawidłowa wzorce w modelu i zawiera przydatne komunikaty.

## <a name="change-tracking"></a>Śledzenie zmian
### <a name="snapshot-change-tracking"></a>Śledzenie zmian migawki
Pozwalają na zmiany w obiektach, aby można było wykryć automatycznie na podstawie porównania ilości bieżący stan kopii (migawki) pierwotnego stanu.
### <a name="notification-change-tracking"></a>Śledzenie zmian powiadomień
Umożliwia jednostki do powiadamiania śledzący zmiany wartości właściwości są modyfikowane.
### <a name="accessing-tracked-state"></a>Uzyskiwanie dostępu do śledzonych stanu
Za pomocą `DbContext.Entry` i `DbContext.ChangeTracker`.
### <a name="attaching-detached-entitiesgraphs"></a>Dołączanie jednostki odłączyć/wykresy
Nowe `DbContext.AttachGraph` interfejsu API pomaga ponownie dołączyć jednostek do kontekstu, aby zapisać nowych/zmodyfikowanych jednostek.

## <a name="saving-data"></a>Zapisywanie danych
### <a name="basic-save-functionality"></a>Podstawowa funkcja zapisywania
Zezwala na zmiany można instancji jednostek utrwalenia w bazie danych.
### <a name="optimistic-concurrency"></a>Optymistycznej współbieżności
Chroni przed nadpisaniem zmian wprowadzonych przez innego użytkownika, ponieważ pobrano dane z bazy danych.
### <a name="async-savechanges"></a>Asynchroniczne metody SaveChanges
Można zwolnić bieżący wątek przetwarzania innych żądań, gdy baza danych przetwarza polecenia wystawiony na podstawie `SaveChanges`.
### <a name="database-transactions"></a>Transakcji bazy danych
Oznacza, że `SaveChanges` jest zawsze niepodzielne (to znaczy albo całkowicie próba powiedzie się lub nie jest zmieniana w bazie danych). Istnieją również transakcji powiązanych interfejsów API umożliwiają udostępnianie transakcji między wystąpieniami kontekstu itp.
### <a name="relational-batching-of-statements"></a>Relacyjne: Przetwarzanie wsadowe instrukcji
Zapewnia lepszą wydajność dzięki przetwarzanie wsadowe zapasową wielu poleceń INSERT/UPDATE/DELETE na jednym obie strony w bazie danych.

## <a name="query"></a>Zapytanie
### <a name="basic-linq-support"></a>Podstawowa pomoc techniczna LINQ
Zapewnia możliwość używania LINQ do pobierania danych z bazy danych.
### <a name="mixed-clientserver-evaluation"></a>Ocena mieszanych klient serwer
Umożliwia zapytania zawiera logikę, która nie można obliczyć w bazie danych i w związku z tym należy ocenić po dane są pobierane do pamięci.
### <a name="notracking"></a>NoTracking
Zapytania umożliwia szybsze wykonywanie zapytania, gdy kontekst nie jest konieczne monitorowanie zmian w wystąpień jednostek (tj. wyniki są tylko do odczytu).
### <a name="eager-loading"></a>Ładowanie wczesny
Udostępnia `Include` i `ThenInclude` metodami do identyfikowania powiązane dane, które mają być pobierane, również podczas wykonywania zapytania.
### <a name="async-query"></a>Zapytania asynchronicznego
Można zwolnić bieżącego wątku (i jego skojarzonych zasobów) do przetwarzania żądań innych podczas bazy danych przetwarza zapytanie.
### <a name="raw-sql-queries"></a>Nieprzetworzona zapytania SQL
Udostępnia `DbSet.FromSql` metodę raw SQL zapytań można pobrać danych. Te zapytania mogą być składane również przy użyciu LINQ.

## <a name="database-schema-management"></a>Zarządzanie schematem bazy danych       
### <a name="database-creationdeletion-apis"></a>Interfejsy API tworzenia/usuwania bazy danych
Przede wszystkim są przeznaczone do testowania, w którym ma zostać szybkie tworzenie/usuwanie bazy danych bez korzystania z migracji.
### <a name="relational-database-migrations"></a>Migracje relacyjnej bazy danych
Zezwalaj na schemacie relacyjnej bazy danych podlegać ewolucji nadgodzinach zmiana modelu.
### <a name="reverse-engineer-from-database"></a>Odtwarzanie z bazy danych
Rusztowania EF model na podstawie schematu relacyjnej bazy danych.

## <a name="database-providers"></a>Dostawcy bazy danych
### <a name="sql-server"></a>SQL Server
Łączy się z programu Microsoft SQL Server 2008 lub nowszym.
### <a name="sqlite"></a>SQLite
Nawiązuje połączenie z bazą danych SQLite 3.
### <a name="in-memory"></a>W pamięci
Umożliwia łatwe testowanie bez nawiązywania połączenia z bazą danych rzeczywistych.
### <a name="3rd-party-providers"></a>3 dostawców
Wielu dostawców są dostępne dla innych baz danych. Zobacz [dostawcy bazy danych](../providers/index.md) pełną listę.
