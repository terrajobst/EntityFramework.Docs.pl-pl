---
title: Co nowego w EF Core 1,0 — EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417525"
---
# <a name="features-included-in-ef-core-10"></a>Funkcje zawarte w EF Core 1,0

## <a name="platforms"></a>Platformy

### <a name="net-framework-451"></a>.NET Framework 4.5.1

Obejmuje konsole, WPF, WinForms, ASP.NET 4 itd.

### <a name="net-standard-13"></a>.NET Standard 1,3

Uwzględnienie ASP.NET Core .NET Framework i .NET Core w systemach Windows, OSX i Linux.

## <a name="modelling"></a>Modelowania

### <a name="basic-modelling"></a>Podstawowe modelowanie

Na podstawie jednostek POCO z właściwościami get/set wspólnych typów skalarnych (`int`, `string`itp.).

### <a name="relationships-and-navigation-properties"></a>Relacje i właściwości nawigacji

Relacje "jeden do wielu" i "jeden do zera" można określić w modelu na podstawie klucza obcego. Właściwości nawigacji prostej kolekcji lub typów referencyjnych mogą być skojarzone z tymi relacjami.

### <a name="built-in-conventions"></a>Konwencje wbudowane

Te kompilują początkowy model na podstawie kształtu klas jednostek.

### <a name="fluent-api"></a>Interfejs API Fluent

Umożliwia przesłonięcie metody `OnModelCreating` w kontekście w celu dodatkowego skonfigurowania modelu, który został odnaleziony przez Konwencję.

### <a name="data-annotations"></a>Adnotacje danych

Są atrybutami, które mogą być dodawane do klas jednostek/właściwości i mają wpływ na model EF. Na przykład dodanie `[Required]` pozwoli na dr znać, że właściwość jest wymagana.

### <a name="relational-table-mapping"></a>Mapowanie tabeli relacyjnej

Zezwala na mapowanie jednostek na tabele/kolumny.

### <a name="key-value-generation"></a>Generowanie wartości klucza

Obejmuje to generowanie po stronie klienta i generowanie bazy danych.

### <a name="database-generated-values"></a>Wartości wygenerowane przez bazę danych

Umożliwia generowanie wartości przez bazę danych przy wstawianiu (wartości domyślne) lub aktualizacji (kolumny obliczane).

### <a name="sequences-in-sql-server"></a>Sekwencje w SQL Server

Umożliwia zdefiniowanie obiektów sekwencji w modelu.

### <a name="unique-constraints"></a>Ograniczenia UNIQUE

Umożliwia definiowanie kluczy alternatywnych i możliwość definiowania relacji przeznaczonych dla tego klucza.

### <a name="indexes"></a>Indeksy

Zdefiniowanie indeksów w modelu powoduje automatyczne wprowadzenie indeksów w bazie danych. Obsługiwane są również unikatowe indeksy.

### <a name="shadow-state-properties"></a>Właściwości stanu cienia

Umożliwia zdefiniowanie właściwości w modelu, które nie są zadeklarowane i nie są przechowywane w klasie .NET, ale mogą być śledzone i aktualizowane przez EF Core. Często używane w przypadku właściwości klucza obcego podczas ujawniania ich w obiekcie nie jest to wymagane.

### <a name="table-per-hierarchy-inheritance-pattern"></a>Wzorzec dziedziczenia na poziomie tabeli

Zezwala, aby jednostki w hierarchii dziedziczenia były zapisywane w pojedynczej tabeli przy użyciu kolumny rozróżniacza, aby identyfikować typy jednostek dla danego rekordu w bazie danych.

### <a name="model-validation"></a>Walidacja modelu

Wykrywa nieprawidłowe wzorce w modelu i udostępnia przydatne komunikaty o błędach.

## <a name="change-tracking"></a>Śledzenie zmian

### <a name="snapshot-change-tracking"></a>Śledzenie zmian migawek

Zezwala na automatyczne wykrywanie zmian w jednostkach, porównując bieżący stan z kopią (migawką) oryginalnego stanu.

### <a name="notification-change-tracking"></a>Śledzenie zmian powiadomień

Umożliwia jednostkom powiadamianie o śledzeniu zmian, gdy wartości właściwości są modyfikowane.

### <a name="accessing-tracked-state"></a>Uzyskiwanie dostępu do śledzonego stanu

Za pośrednictwem `DbContext.Entry` i `DbContext.ChangeTracker`.

### <a name="attaching-detached-entitiesgraphs"></a>Dołączanie odłączonych jednostek/wykresów

Nowy interfejs API `DbContext.AttachGraph` pomaga ponownie dołączyć jednostki do kontekstu w celu zapisania nowych/zmodyfikowanych jednostek.

## <a name="saving-data"></a>Zapisywanie danych

### <a name="basic-save-functionality"></a>Podstawowe funkcje zapisywania

Zezwala na utrwalanie zmian wystąpień jednostek w bazie danych.

### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność

Chroni przed zastąpieniem zmian wprowadzonych przez innego użytkownika od momentu pobrania danych z bazy danych.

### <a name="async-savechanges"></a>Metody SaveChanges Async

Może zwolnić bieżący wątek, aby przetwarzać inne żądania, podczas gdy baza danych przetwarza polecenia wydane z `SaveChanges`.

### <a name="database-transactions"></a>Transakcje bazy danych

Oznacza, że `SaveChanges` jest zawsze niepodzielna (oznacza to, że całkowicie kończy się powodzeniem lub nie wprowadzono żadnych zmian w bazie danych). Istnieją również interfejsy API powiązane z transakcjami umożliwiające udostępnianie transakcji między wystąpieniami kontekstu itd.

### <a name="relational-batching-of-statements"></a>Relacyjne: przetwarzanie wsadowe instrukcji

Zapewnia lepszą wydajność dzięki zbiorom wsadowym wielokrotnego wstawiania/aktualizowania/usuwania w jednej komunikacji jednokierunkowej z bazą danych.

## <a name="query"></a>Zapytanie

### <a name="basic-linq-support"></a>Podstawowa obsługa LINQ

Umożliwia korzystanie z programu LINQ do pobierania danych z bazy danych.

### <a name="mixed-clientserver-evaluation"></a>Mieszane Obliczanie klienta/serwera

Włącza zapytania, które mają zawierać logikę, której nie można oszacować w bazie danych i dlatego muszą być oceniane po pobraniu danych do pamięci.

### <a name="notracking"></a>NoTracking

Zapytania umożliwiają szybsze wykonywanie zapytań, gdy kontekst nie musi monitorować zmian w wystąpieniach jednostek (jest to przydatne, jeśli wyniki są tylko do odczytu).

### <a name="eager-loading"></a>Ładowanie eager

Udostępnia metody `Include` i `ThenInclude` do identyfikowania powiązanych danych, które powinny być również pobierane podczas wykonywania zapytań.

### <a name="async-query"></a>Zapytanie asynchroniczne

Może zwolnić bieżący wątek (i skojarzone zasoby), aby przetwarzać inne żądania, gdy baza danych przetwarza zapytanie.

### <a name="raw-sql-queries"></a>Nieprzetworzone zapytania SQL

Udostępnia metodę `DbSet.FromSql` do pobierania danych przy użyciu nieprzetworzonych zapytań SQL. Te zapytania mogą również składać się z użycia LINQ.

## <a name="database-schema-management"></a>Zarządzanie schematami bazy danych

### <a name="database-creationdeletion-apis"></a>Interfejsy API tworzenia/usuwania bazy danych

Są głównie przeznaczone do testowania, gdzie chcesz szybko utworzyć/usunąć bazę danych bez użycia migracji.

### <a name="relational-database-migrations"></a>Migracje relacyjnych baz danych

Zezwól schematowi relacyjnej bazy danych na rozwijanie nadgodzin w miarę zmiany modelu.

### <a name="reverse-engineer-from-database"></a>Odtwarzanie z bazy danych

Szkieletuje model EF oparty na istniejącym schemacie relacyjnej bazy danych.

## <a name="database-providers"></a>Dostawcy baz danych

### <a name="sql-server"></a>Oprogramowanie SQL Server

Nawiązuje połączenie z Microsoft SQL Server 2008.

### <a name="sqlite"></a>SQLite

Nawiązuje połączenie z bazą danych programu SQLite 3.

### <a name="in-memory"></a>W pamięci

Program został zaprojektowany tak, aby można było łatwo włączyć testowanie bez łączenia się z rzeczywistą bazą danych.

### <a name="3rd-party-providers"></a>dostawcy innych firm

Kilku dostawców jest dostępnych dla innych aparatów bazy danych. Aby uzyskać pełną listę, zobacz [dostawcy bazy danych](../providers/index.md) .
