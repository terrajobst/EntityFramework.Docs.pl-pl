---
title: Co nowego w EF Core 1.0 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417525"
---
# <a name="features-included-in-ef-core-10"></a>Funkcje zawarte w EF Core 1.0

## <a name="platforms"></a>Platformy

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Zawiera konsolę, WPF, WinForms, ASP.NET 4 itp.

### <a name="net-standard-13"></a>.NET Standard 1.3

W tym ASP.NET Core kierowania zarówno .NET Framework i .NET Core w systemie Windows, OSX i Linux.

## <a name="modelling"></a>Modelowanie

### <a name="basic-modelling"></a>Modelowanie podstawowe

Na podstawie jednostek POCO z właściwościami get/set`int` `string`typowych typów skalarnych ( , itp.).

### <a name="relationships-and-navigation-properties"></a>Relacje i właściwości nawigacji

Relacje jeden do wielu i jeden do zera lub jeden można określić w modelu na podstawie klucza obcego. Właściwości nawigacji typów kolekcji prostej lub odwołania mogą być skojarzone z tymi relacjami.

### <a name="built-in-conventions"></a>Wbudowane konwencje

Te kompilacji modelu początkowego na podstawie kształtu klas jednostki.

### <a name="fluent-api"></a>Płynne API

Umożliwia zastąpienie `OnModelCreating` metody w kontekście, aby dodatkowo skonfigurować model, który został wykryty przez konwencję.

### <a name="data-annotations"></a>Adnotacje danych

Są atrybuty, które mogą być dodawane do klasy/właściwości jednostki i będzie mieć wpływ na model EF. Na przykład `[Required]` dodanie poinformuje EF, że właściwość jest wymagana.

### <a name="relational-table-mapping"></a>Mapowanie tabeli relacyjnej

Umożliwia mapowanie elementów do tabel/kolumn.

### <a name="key-value-generation"></a>Generowanie kluczowych wartości

W tym generowanie po stronie klienta i generowanie bazy danych.

### <a name="database-generated-values"></a>Wartości wygenerowane przez bazę danych

Umożliwia generowanie wartości przez bazę danych przy wstawiania (wartości domyślne) lub aktualizacji (kolumny obliczane).

### <a name="sequences-in-sql-server"></a>Sekwencje w programie SQL Server

Umożliwia definiowanie obiektów sekwencji w modelu.

### <a name="unique-constraints"></a>Unique

Umożliwia definicję kluczy alternatywnych i możliwość definiowania relacji, które są przeznaczone dla tego klucza.

### <a name="indexes"></a>Indeksy

Definiowanie indeksów w modelu automatycznie wprowadza indeksy w bazie danych. Obsługiwane są również unikatowe indeksy.

### <a name="shadow-state-properties"></a>Właściwości stanu cienia

Umożliwia zdefiniowanie właściwości w modelu, które nie są zadeklarowane i nie są przechowywane w klasie .NET, ale mogą być śledzone i aktualizowane przez EF Core. Często używane dla właściwości klucza obcego podczas wystawiania ich w obiekcie nie jest pożądane.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Wzorzec dziedziczenia tabela na hierarchię

Umożliwia zapisywanie jednostek w hierarchii dziedziczenia w jednej tabeli przy użyciu kolumny rozróżniacza w celu zidentyfikowania ich typu jednostki dla danego rekordu w bazie danych.

### <a name="model-validation"></a>Walidacja modelu

Wykrywa nieprawidłowe wzorce w modelu i udostępnia przydatne komunikaty o błędach.

## <a name="change-tracking"></a>Śledzenie zmian

### <a name="snapshot-change-tracking"></a>Śledzenie zmian migawek

Umożliwia automatyczne wykrywanie zmian w jednostkach przez porównanie bieżącego stanu z kopią (migawką) stanu oryginalnego.

### <a name="notification-change-tracking"></a>Śledzenie zmian powiadomień

Umożliwia jednostkom powiadamianie śledzenia zmian, gdy wartości właściwości są modyfikowane.

### <a name="accessing-tracked-state"></a>Uzyskiwanie dostępu do śledzonego stanu

Przez `DbContext.Entry` `DbContext.ChangeTracker`i .

### <a name="attaching-detached-entitiesgraphs"></a>Dołączanie odłączonych elementów/wykresów

Nowy `DbContext.AttachGraph` interfejs API pomaga ponownie dołączyć jednostki do kontekstu w celu zapisania nowych/zmodyfikowanych jednostek.

## <a name="saving-data"></a>Zapisywanie danych

### <a name="basic-save-functionality"></a>Podstawowa funkcja zapisywania

Umożliwia utrwalenie zmian w wystąpieniach encji w bazie danych.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Chroni przed zastąpieniem zmian wprowadzonych przez innego użytkownika, ponieważ dane zostały pobrane z bazy danych.

### <a name="async-savechanges"></a>Async SaveChanges

Można zwolnić bieżący wątek do przetwarzania innych żądań, `SaveChanges`podczas gdy baza danych przetwarza polecenia wydane z .

### <a name="database-transactions"></a>Transakcje bazy danych

Oznacza, `SaveChanges` że jest zawsze niepodzielny (co oznacza, że albo całkowicie powiedzie się lub nie wprowadza się żadnych zmian w bazie danych). Istnieją również interfejsy API związane z transakcjami, aby umożliwić udostępnianie transakcji między wystąpieniami kontekstu itp.

### <a name="relational-batching-of-statements"></a>Relacyjne: Wsadowanie instrukcji

Zapewnia lepszą wydajność przez wsadowanie wiele wstawiania/aktualizacji / usuwania poleceń w jednym obie strony do bazy danych.

## <a name="query"></a>Zapytanie

### <a name="basic-linq-support"></a>Podstawowa obsługa LINQ

Zapewnia możliwość korzystania z LINQ do pobierania danych z bazy danych.

### <a name="mixed-clientserver-evaluation"></a>Ocena klienta/serwera mieszanego

Umożliwia kwerendy zawierają logikę, która nie może być oceniana w bazie danych i dlatego muszą być oceniane po pobraniu danych do pamięci.

### <a name="notracking"></a>Notracking

Kwerendy umożliwia szybsze wykonywanie zapytań, gdy kontekst nie musi monitorować zmian w wystąpieniach jednostki (jest to przydatne, jeśli wyniki są tylko do odczytu).

### <a name="eager-loading"></a>Gorliwy załadunek

Zawiera `Include` metody `ThenInclude` i metody identyfikowania powiązanych danych, które również powinny być pobierane podczas wykonywania zapytań.

### <a name="async-query"></a>Zapytanie asynchroniowe

Można zwolnić bieżący wątek (i jest skojarzone zasoby) do przetwarzania innych żądań, podczas gdy baza danych przetwarza kwerendę.

### <a name="raw-sql-queries"></a>Nieprzetworzone zapytania SQL

Udostępnia `DbSet.FromSql` metodę używania nieprzetworzonych zapytań SQL do pobierania danych. Te zapytania mogą również składać się przy użyciu LINQ.

## <a name="database-schema-management"></a>Zarządzanie schematem bazy danych

### <a name="database-creationdeletion-apis"></a>Interfejsy API tworzenia/usuwania bazy danych

Są przeznaczone głównie do testowania, gdzie chcesz szybko utworzyć/usunąć bazę danych bez użycia migracji.

### <a name="relational-database-migrations"></a>Migracje relacyjnej bazy danych

Zezwalaj na schemat relacyjnej bazy danych, aby ewoluować nadgodziny w miarę zmian modelu.

### <a name="reverse-engineer-from-database"></a>Inżynier odwrotny z bazy danych

Szkielety modelu EF na podstawie istniejącego schematu relacyjnej bazy danych.

## <a name="database-providers"></a>Dostawcy baz danych

### <a name="sql-server"></a>SQL Server

Łączy się z programem Microsoft SQL Server 2008.

### <a name="sqlite"></a>SQLite

Łączy się z bazą danych SQLite 3.

### <a name="in-memory"></a>W pamięci

Został zaprojektowany, aby łatwo włączyć testowanie bez łączenia się z prawdziwą bazą danych.

### <a name="3rd-party-providers"></a>Dostawcy zewnętrzni

Kilku dostawców są dostępne dla innych aparatów baz danych. Zobacz [dostawców baz danych,](../providers/index.md) aby uzyskać pełną listę.
