---
title: Przenoszenie z programu EF6 do programu EF Core — sprawdzanie poprawności wymagań
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993443"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Przed przenoszenie z programu EF6 do programu EF Core: Sprawdzanie poprawności wymagań aplikacji

Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy programu EF Core spełnia wymagania dotyczące dostępu do danych aplikacji.

## <a name="missing-features"></a>Brak funkcji

Upewnij się, że programu EF Core zawiera wszystkie funkcje potrzebne do użycia w aplikacji. Zobacz [porównanie funkcji](../features.md) szczegółowe porównanie porównanie zestaw w wersji EF Core funkcji platformy EF6. Jeśli brakuje dowolnej wymagane funkcje, upewnij się, że możesz kompensuje braku tych funkcji, przed eksportowanie do programu EF Core.

## <a name="behavior-changes"></a>Zmiany zachowania

To jest niepełna lista zmian w zachowaniu między EF6 i EF Core. Należy zachować te należy pamiętać, jako port aplikacji zgodnie z ich może zmienić sposób, w jaki aplikacja działa, ale nie będą wyświetlane jako błędy kompilacji po zamianie do programu EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Zachowanie DbSet.Add/Attach i wykres

W EF6 wywołanie `DbSet.Add()` na jednostki skutkuje Wyszukiwanie cykliczne dla wszystkich jednostek, do którego odwołuje się jego właściwości nawigacji. Wszystkie jednostki, które znajdują się i nie są już śledzone przez kontekście, są również oznaczane w miarę dodawania. `DbSet.Attach()` zachowuje się tak samo, z wyjątkiem wszystkie jednostki są oznaczone jako niezmieniony.

**EF Core wykonuje wyszukiwanie cykliczne podobne, ale niektóre nieco inne reguły.**

*  Jednostka główna jest zawsze żądany stan (dodane do `DbSet.Add` bez zmian dla `DbSet.Attach`).

*  **W przypadku jednostek, które zostały znalezione w czasie cykliczne wyszukiwanie właściwości nawigacji:**

    *  **Jeśli klucz podstawowy jednostki jest generowany magazynu**

        * Jeśli nie ustawiono klucza podstawowego z wartością, stan jest ustawiony do dodanych. Wartość klucza podstawowego jest uznawany za "nie jest ustawiona" Jeśli jest ona przypisana wartość domyślna CLR dla typu właściwości (na przykład `0` dla `int`, `null` dla `string`itp.).

        * Jeśli klucz podstawowy jest ustawiona na wartość, stan jest ustawiony bez zmian.

    *  Jeśli klucz podstawowy nie jest generowany w bazie danych, jednostka jest umieszczany w tym samym stanie, jako katalog główny.

### <a name="code-first-database-initialization"></a>Kod pierwszy inicjowanie bazy danych

**EF6 ma znacznej ilości magic, który wykonuje wokół wybranie połączenia z bazą danych i inicjowania bazy danych. Oto niektóre z tych reguł:**

* Jeśli konfiguracja nie jest wykonywana, EF6 wybierze bazę danych programu SQL Express lub LocalDb.

* Jeśli parametry połączenia z taką samą nazwę jak kontekstu znajduje się w aplikacji `App/Web.config` pliku, to połączenie będzie używane.

* Jeśli baza danych nie istnieje, zostanie utworzony.

* Jeśli żadna z tabel z modelu istnieje w bazie danych, schematów dla bieżącego modelu jest dodawany do bazy danych. Jeśli migracja jest włączona, następnie służą one do utworzenia bazy danych.

* Jeśli baza danych istnieje i EF6 poprzednio utworzono schemat, schemat jest sprawdzane pod kątem zgodności z bieżącego modelu. Wyjątek jest generowany, jeśli model został zmieniony od czasu utworzenia schematu.

**EF Core nie wykonuje tego magic.**

* Połączenie z bazą danych muszą być jawnie skonfigurowane w kodzie.

* Inicjowanie nie jest wykonywane. Należy użyć `DbContext.Database.Migrate()` do zastosowania migracji (lub `DbContext.Database.EnsureCreated()` i `EnsureDeleted()` do tworzenia/usuwania bazy danych bez używania migracji).

### <a name="code-first-table-naming-convention"></a>Pierwsza tabela kodu konwencje nazewnictwa

EF6 uruchamia Nazwa klasy jednostki za pośrednictwem usługi pluralizacja do obliczania jednostki jest mapowany na domyślną nazwę tabeli.

EF Core używa nazwy `DbSet` właściwości jednostki jest widoczna w w kontekście pochodnych. Jeśli jednostka ma `DbSet` właściwości, a następnie nazwę klasy jest używany.
