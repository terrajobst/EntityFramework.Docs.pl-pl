---
title: "Eksportowanie z EF6 do EF Core - weryfikacji wymagań"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Przed eksportowanie z EF6 do EF Core: Sprawdzanie poprawności wymagań aplikacji

Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy podstawowe EF spełnia wymagania dotyczące dostępu do danych aplikacji.

## <a name="missing-features"></a>Brak funkcji

Upewnij się, że podstawowe EF ma wszystkich funkcji potrzebnych do użycia w aplikacji. Zobacz [porównanie funkcji](../features.md) szczegółowe porównanie jak porównuje EF6 zestawu w EF podstawowych funkcji. Brakuje niektórych wymaganych funkcji, upewnij się, że można zniwelować braku te funkcje przed eksportowanie do EF Core.

## <a name="behavior-changes"></a>Zmiany sposobu działania

Jest niepełna lista pewne zmiany w zachowaniu między EF6 i EF podstawowe. Należy zachować te pamiętać jako port aplikacji, ponieważ mogą one ulec zmianie sposób aplikacja działa, ale nie będą widoczne jako błędy kompilacji po wymiany na EF rdzeń.

### <a name="dbsetaddattach-and-graph-behavior"></a>Wykres i DbSet.Add/Attach zachowanie

W EF6 wywoływania `DbSet.Add()` na powoduje cykliczne wyszukiwanie wszystkich jednostek, do którego odwołuje się jego właściwości nawigacji jednostki. Wszystkie jednostki, które zostały znalezione i nie są już śledzone przez kontekst, również są oznaczone jako dodane. `DbSet.Attach()`zachowuje się tak samo, z wyjątkiem wszystkie jednostki są oznaczone jako bez zmian.

**EF Core wykonuje podobne Wyszukiwanie cykliczne, ale niektóre nieco inne reguły.**

*  Obiekt główny jest zawsze w żądany stan (dodany do `DbSet.Add` bez zmian dla `DbSet.Attach`).

*  **Dla obiektów, które zostały znalezione podczas cyklicznego wyszukiwania właściwości nawigacji:**

    *  **W przypadku klucza podstawowego jednostki wygenerowane**

        * Jeśli klucz podstawowy nie jest ustawiona na wartość, stan jest ustawiony do dodany. Wartość klucza podstawowego jest uznawany za "nie jest ustawiona" przypisane wartości domyślne CLR dla typu właściwości (tj. `0` dla `int`, `null` dla `string`itp.).

        * Jeśli klucz podstawowy jest ustawiona na wartość, stan ustawiono bez zmian.

    *  Jeśli klucz podstawowy nie jest generowany przez bazę danych, jednostka jest umieszczany w tym samym stanie, jako element główny.

### <a name="code-first-database-initialization"></a>Kod pierwszego inicjowania bazy danych

**EF6 ma dużą magic, którą wykonuje wokół wybranie połączenia z bazą danych i inicjowania bazy danych. Oto niektóre z tych reguł:**

* Jeśli konfiguracja nie jest wykonywana, EF6 wybierze bazy danych programu SQL Express lub LocalDb.

* Jeśli parametry połączenia o tej samej nazwie jako kontekst jest w aplikacjach `App/Web.config` plików, to połączenie będzie używane.

* Jeśli baza danych nie istnieje, jest tworzony.

* Jeśli żadna z tabel z modelu nie istnieją w bazie danych, schematu dla bieżącego modelu jest dodawany do bazy danych. Jeśli włączono migracje, następnie są one używane do utworzenia bazy danych.

* Jeśli baza danych istnieje i EF6 wcześniej utworzyć schemat, schemat jest sprawdzane pod kątem zgodności z bieżącym modelem. Jeśli model został zmieniony od czasu utworzenia schematu, jest zgłaszany wyjątek.

**Podstawowe EF nie wykonuje tego magic.**

* Połączenie z bazą danych musi być jawnie skonfigurowany w kodzie.

* Inicjowanie nie jest wykonywana. Należy użyć `DbContext.Database.Migrate()` do zastosowania migracji (lub `DbContext.Database.EnsureCreated()` i `EnsureDeleted()` do tworzenia/usuwania bazy danych bez korzystania z migracji).

### <a name="code-first-table-naming-convention"></a>Kod konwencji nazewnictwa pierwszej tabeli.

EF6 uruchamia Nazwa klasy jednostki za pośrednictwem usługi określania liczby mnogiej do obliczenia jednostka jest zamapowana na domyślna nazwa tabeli.

Podstawowe EF używa nazwy `DbSet` właściwości jednostki jest widoczna w pochodnej kontekstu. Jeśli taka jednostka nie posiada `DbSet` właściwości, a następnie nazwę klasy jest używany.
