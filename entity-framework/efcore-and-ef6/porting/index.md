---
title: Przenoszenie z EF6 do EF Core - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 77096b9bffba6b8c2a3d7bfb0c2e41e2d170a7db
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416951"
---
# <a name="porting-from-ef6-to-ef-core"></a>Przenoszenie z programu EF6 do programu EF Core

Ze względu na podstawowe zmiany w EF Core nie zaleca się próby przeniesienia aplikacji EF6 do EF Core, chyba że masz istotny powód, aby wprowadzić zmiany.
Należy wyświetlić przejście z EF6 do EF Core jako port, a nie uaktualnienie.

> [!IMPORTANT]
> Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy ef core spełnia wymagania dotyczące dostępu do danych dla aplikacji.

## <a name="missing-features"></a>Brakujące funkcje

Upewnij się, że EF Core ma wszystkie funkcje, które należy użyć w aplikacji. Zobacz [porównanie funkcji, aby](xref:efcore-and-ef6/index) uzyskać szczegółowe porównanie sposobu porównywania funkcji w ef core z EF6. Jeśli brakuje żadnych wymaganych funkcji, upewnij się, że można zrekompensować brak tych funkcji przed przeniesieniem do EF Core.

## <a name="behavior-changes"></a>Zmiany w zachowaniu

Jest to niewyczerpywna lista niektórych zmian w zachowaniu między EF6 i EF Core. Ważne jest, aby zachować te na uwadze jako portu aplikacji, ponieważ mogą one zmienić sposób zachowania aplikacji, ale nie pojawi się jako błędy kompilacji po zamianie do EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach i zachowanie wykresu

W EF6 `DbSet.Add()` wywołanie jednostki powoduje cykliczne wyszukiwanie dla wszystkich jednostek, do których odwołuje się jego właściwości nawigacji. Wszystkie jednostki, które zostały znalezione i nie są już śledzone przez kontekst, są również oznaczone jako dodane. `DbSet.Attach()`zachowuje się tak samo, z wyjątkiem wszystkich jednostek są oznaczone jako niezmienione.

**EF Core wykonuje podobne wyszukiwanie cykliczne, ale z niektórymi nieco innymi regułami.**

*  Jednostka główna jest zawsze w żądanym `DbSet.Add` stanie (dodane `DbSet.Attach`dla i bez zmian dla ).

*  **Dla jednostek, które znajdują się podczas wyszukiwania cyklicznego właściwości nawigacji:**

    *  **Jeśli klucz podstawowy jednostki jest magazynowy generowany**

        * Jeśli klucz podstawowy nie jest ustawiony na wartość, stan jest ustawiony na dodane. Wartość klucza podstawowego jest uważana za "nie ustawioną", jeśli jest przypisana `0` `int`wartość `null` `string`domyślna CLR dla typu właściwości (na przykład dla , for , itp.).

        * Jeśli klucz podstawowy jest ustawiony na wartość, stan jest ustawiony na niezmienionym.

    *  Jeśli klucz podstawowy nie jest generowany przez bazę danych, jednostka jest umieszczana w tym samym stanie co katalog główny.

### <a name="code-first-database-initialization"></a>Inicjowanie bazy danych Code First

**EF6 ma znaczną ilość magii wykonuje wokół wybierania połączenia bazy danych i inicjowania bazy danych. Niektóre z tych zasad obejmują:**

* Jeśli nie zostanie wykonana żadna konfiguracja, EF6 wybierze bazę danych w programie SQL Express lub LocalDb.

* Jeśli ciąg połączenia o takiej samej nazwie `App/Web.config` jak kontekst znajduje się w pliku aplikacji, to połączenie będzie używane.

* Jeśli baza danych nie istnieje, jest tworzona.

* Jeśli żadna z tabel z modelu istnieje w bazie danych, schemat dla bieżącego modelu jest dodawany do bazy danych. Jeśli migracje są włączone, są one używane do tworzenia bazy danych.

* Jeśli baza danych istnieje i EF6 wcześniej utworzył schemat, schemat jest sprawdzany pod kątem zgodności z bieżącym modelem. Wyjątek jest zgłaszany, jeśli model uległ zmianie od czasu utworzenia schematu.

**EF Core nie wykonuje żadnej z tej magii.**

* Połączenie z bazą danych musi być jawnie skonfigurowane w kodzie.

* Nie jest wykonywana inicjalizacja. Należy użyć `DbContext.Database.Migrate()` do zastosowania migracji `DbContext.Database.EnsureCreated()` `EnsureDeleted()` (lub i utworzyć/usunąć bazę danych bez użycia migracji).

### <a name="code-first-table-naming-convention"></a>Konwencja nazewnictwa pierwszej tabeli kodu

EF6 uruchamia nazwę klasy jednostki za pośrednictwem usługi pluralizacji, aby obliczyć domyślną nazwę tabeli, do której jednostka jest mapowana.

EF Core używa nazwy `DbSet` właściwości, która jest widoczna w kontekście pochodnym. Jeśli jednostka nie ma `DbSet` właściwości, używana jest nazwa klasy.
