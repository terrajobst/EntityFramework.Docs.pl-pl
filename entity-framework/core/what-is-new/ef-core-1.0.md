---
title: What's new in EF Core 1.0 — EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: af7cf490ef2b04afb02461279fbe67c1c7fa3d95
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949025"
---
# <a name="features-included-in-ef-core-10"></a>Funkcje zawarte w programie EF Core 1.0

## <a name="platforms"></a>Platformy
### <a name="net-framework-451"></a>.NET Framework 4.5.1
Obejmuje konsolę, WPF, WinForms, platformy ASP.NET 4, itd.
### <a name="net-standard-13"></a>.NET standard 1.3
W tym platformy ASP.NET Core przeznaczone dla .NET Framework i .NET Core w Windows, OS x i Linux.

## <a name="modelling"></a>Modelowanie
### <a name="basic-modelling"></a>Podstawowego modelowania
Oparte na jednostkach POCO za pomocą właściwości get/set popularnych typów skalarnych (`int`, `string`itp.).
### <a name="relationships-and-navigation-properties"></a>Relacje i właściwości nawigacji
W modelu, w oparciu o klucz obcy można określić jeden do wielu i relacje jeden do zero lub jeden. Właściwości nawigacji typów prostych kolekcji lub odwołania może być skojarzony z tych relacji.
### <a name="built-in-conventions"></a>Konwencje wbudowane
Te tworzenie początkowej model oparty na kształcie klas jednostek.
### <a name="fluent-api"></a>Interfejs Fluent API
Zezwala na zastąpienie `OnModelCreating` metody na kontekst dodatkowo skonfigurować modelu, która została wykryta przez Konwencję.
### <a name="data-annotations"></a>Adnotacje danych
To atrybutów, można dodać do swojej klasy/właściwości jednostki, które wpływają na modelu platformy EF. Na przykład dodanie `[Required]` umożliwi EF wiedzieć, że właściwość jest wymagana.
### <a name="relational-table-mapping"></a>Relacyjne Mapowanie tabeli
Umożliwia jednostek, które mają być mapowane do tabele/kolumny.
### <a name="key-value-generation"></a>Generowanie wartości klucza
W tym generowania po stronie klienta i generowanie bazy danych.
### <a name="database-generated-values"></a>Baza danych generowane wartości
Zezwala na wartości, które mają być generowane przez bazę danych na wstawiania (wartości domyślne) lub aktualizacji (kolumn obliczanych).
### <a name="sequences-in-sql-server"></a>Sekwencje w programie SQL Server
Umożliwia sekwencji obiektów zdefiniowanych w modelu.
### <a name="unique-constraints"></a>Unikatowych ograniczeń
Umożliwia określenie klucze alternatywne i możliwość definiowania relacji przeznaczonych tego klucza.
### <a name="indexes"></a>Indeksy
Definiowanie indeksów w modelu automatycznie wprowadza indeksy w bazie danych. Unikatowe indeksy są również obsługiwane.
### <a name="shadow-state-properties"></a>Właściwości stanu w tle
Umożliwia właściwości zdefiniowanych w modelu, które nie są deklarowane i nie są przechowywane w klasie .NET, ale mogą być śledzone i aktualizowane przez platformę EF Core. Często używane do właściwości klucza obcego, gdy udostępnianie w obiekt nie jest wymagana.
### <a name="table-per-hierarchy-inheritance-pattern"></a>Tabela wg hierarchii dziedziczenia wzorzec
Umożliwia jednostek w hierarchii dziedziczenia do zapisania pojedynczej tabeli, aby zidentyfikować ich typ jednostki dla danego rekordu w bazie danych przy użyciu kolumna dyskryminatora.
### <a name="model-validation"></a>Walidacja modelu
Wykrywa nieprawidłowe wzorców w modelu i zawiera komunikaty o błędach pomocne.

## <a name="change-tracking"></a>Śledzenie zmian
### <a name="snapshot-change-tracking"></a>Śledzenie zmian migawki
Umożliwia zmiany w jednostkach, aby zostało wykryte automatycznie przez porównanie bieżącego stanu pierwotnego stanu kopii (Migawka).
### <a name="notification-change-tracking"></a>Śledzenie zmian powiadomień
Umożliwia jednostek do powiadamiania śledzenie zmian wartości właściwości są modyfikowane.
### <a name="accessing-tracked-state"></a>Uzyskiwanie dostępu do śledzonych stanu
Za pomocą `DbContext.Entry` i `DbContext.ChangeTracker`.
### <a name="attaching-detached-entitiesgraphs"></a>Dołączanie odłączonych jednostek/wykresów
Nowy `DbContext.AttachGraph` interfejs API pomaga ponownie podłącz jednostek do kontekstu w celu zapisania nowych/zmodyfikowanych jednostek.

## <a name="saving-data"></a>Zapisywanie danych
### <a name="basic-save-functionality"></a>Podstawowa funkcja zapisywania
Umożliwia zmiany wystąpień jednostek w celu jego utrwalenia w bazie danych.
### <a name="optimistic-concurrency"></a>Optymistyczna współbieżność
Chroni przed zastępowanie zmian wprowadzonych przez innego użytkownika, ponieważ dane została pobrana z bazy danych.
### <a name="async-savechanges"></a>Async SaveChanges
Zwolnić bieżącego wątku przetwarzanie innych żądań, gdy baza danych przetwarza poleceń wydawanych z `SaveChanges`.
### <a name="database-transactions"></a>Transakcje bazy danych
Oznacza, że `SaveChanges` jest zawsze niepodzielne (tzn. jej albo całkowicie zakończy się pomyślnie, lub są wprowadzane żadne zmiany w bazie danych). Istnieją również transakcji powiązanych interfejsów API, aby zezwolić na udostępnianie transakcji między wystąpieniami kontekstu itp.
### <a name="relational-batching-of-statements"></a>Relacyjne: Przetwarzanie wsadowe instrukcji
Zapewnia lepszą wydajność, przetwarzanie wsadowe się wielu poleceń WSTAWIANIA/AKTUALIZOWANIA/usuwania do pojedynczego obie strony do bazy danych.

## <a name="query"></a>Zapytanie
### <a name="basic-linq-support"></a>Podstawowa pomoc techniczna LINQ
Zapewnia możliwość korzystania z LINQ do pobierania danych z bazy danych.
### <a name="mixed-clientserver-evaluation"></a>Ocena mieszane klient serwer
Umożliwia zapytaniom zawiera logikę, która nie może być ocenione w bazie danych, a w związku z tym muszą być oceniane, po pobraniu danych do pamięci.
### <a name="notracking"></a>NoTracking
Zapytania umożliwia szybsze wykonywanie zapytania, gdy kontekst nie jest konieczne monitorowanie zmian w wystąpień jednostek (jest to przydatne, gdy wyniki są tylko do odczytu).
### <a name="eager-loading"></a>Wczesne ładowanie
Udostępnia `Include` i `ThenInclude` metodami do identyfikowania powiązanych danych, które również mają być pobierane, podczas wykonywania zapytania.
### <a name="async-query"></a>Zapytania asynchroniczne
Można zwolnić miejsce na bieżącego wątku (i jego skojarzone zasoby) do przetworzenia inne żądania, gdy baza danych przetwarza zapytania.
### <a name="raw-sql-queries"></a>Pierwotne zapytania SQL
Udostępnia `DbSet.FromSql` metodę SQL pierwotne zapytania można pobrać danych. Te zapytania może składać się także na temat korzystania z LINQ.

## <a name="database-schema-management"></a>Zarządzanie schematami bazy danych       
### <a name="database-creationdeletion-apis"></a>Baza danych tworzenia/usuwania interfejsów API
Przede wszystkim są przeznaczone do testowania, które chcesz szybko tworzyć/usuwać bazy danych bez używania migracji.
### <a name="relational-database-migrations"></a>Migracje relacyjnej bazy danych
Zezwalaj na schemat relacyjnej bazy danych w rozwój nadgodzinach jako zmiany modelu.
### <a name="reverse-engineer-from-database"></a>Odtwarzanie z bazy danych
Szkielety mechanizmów modelu platformy EF oparte na istniejący schemat relacyjnej bazy danych.

## <a name="database-providers"></a>Dostawcy baz danych
### <a name="sql-server"></a>SQL Server
Łączy się z programu Microsoft SQL Server 2008 lub nowszym.
### <a name="sqlite"></a>Bazy danych SQLite
Nawiązuje połączenie z bazą danych SQLite 3.
### <a name="in-memory"></a>W pamięci
Jest umożliwiających łatwe testowanie bez połączenia z prawdziwą bazą danych.
### <a name="3rd-party-providers"></a>3 dostawców
Kilku dostawców są dostępne dla innych baz danych. Zobacz [dostawcy baz danych](../providers/index.md) pełną listę.
