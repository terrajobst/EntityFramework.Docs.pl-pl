---
title: Przenoszenie od EF6 do EF Core-Weryfikuj wymagania
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565344"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Przed przystąpieniem do przenoszenia z EF6 do EF Core: Weryfikowanie wymagań aplikacji

Przed rozpoczęciem procesu przenoszenia należy sprawdzić, czy EF Core spełnia wymagania dotyczące dostępu do danych aplikacji.

## <a name="missing-features"></a>Brakujące funkcje

Upewnij się, że EF Core ma wszystkie funkcje, których potrzebujesz użyć w aplikacji. Zobacz [porównanie funkcji](../features.md) , aby uzyskać szczegółowe informacje na temat sposobu, w jaki zestaw funkcji w EF Core jest porównywany z Ef6. Jeśli brakuje wymaganych funkcji, upewnij się, że można wyrównać brak tych funkcji przed rozpoczęciem przenoszenia do EF Core.

## <a name="behavior-changes"></a>Zmiany zachowania

To nie jest wyczerpująca lista niektórych zmian w zachowaniu między EF6 i EF Core. Ważne jest, aby zachować te kwestie jako portów, które mogą zmienić sposób działania aplikacji, ale nie będą wyświetlane jako błędy kompilacji po zamianie na EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Nieogólnymi. Add/Attach i Graph Behavior

W Ef6 wywoływanie `DbSet.Add()` jednostki powoduje cykliczne wyszukiwanie wszystkich jednostek, do których odwołuje się we właściwościach nawigacji. Wszystkie znalezione jednostki i nie są już śledzone przez kontekst, również są oznaczone jako dodane. `DbSet.Attach()`zachowuje się tak samo, chyba że wszystkie jednostki są oznaczone jako niezmienione.

**EF Core wykonuje podobne wyszukiwanie cykliczne, ale przy niewielkich różnych regułach.**

*  Jednostka główna jest zawsze w żądanym stanie (dodana do `DbSet.Add` i niezmieniona dla `DbSet.Attach`).

*  **Dla jednostek znalezionych podczas cyklicznego wyszukiwania właściwości nawigacji:**

    *  **Jeśli klucz podstawowy jednostki jest generowany przez magazyn**

        * Jeśli klucz podstawowy nie jest ustawiony na wartość, stan jest ustawiany na dodane. Wartość klucza podstawowego jest uznawana za "nie ustawiono", jeśli jest przypisana wartość domyślna CLR dla typu właściwości ( `0` na przykład `null` dla `int` `string`, dla, itp.).

        * Jeśli klucz podstawowy ma ustawioną wartość, stan jest ustawiony na niezmienione.

    *  Jeśli klucz podstawowy nie jest generowany w bazie danych, jednostka zostanie umieszczona w tym samym stanie co główny.

### <a name="code-first-database-initialization"></a>Code First inicjalizacji bazy danych

**EF6 ma znaczną ilość Magic, która wykonuje wokół wyboru połączenia bazy danych i zainicjowania bazy danych. Niektóre z tych reguł obejmują:**

* Jeśli konfiguracja nie zostanie wykonana, EF6 wybierze bazę danych w programie SQL Express lub LocalDb.

* Jeśli parametr połączenia o takiej samej nazwie jak kontekst znajduje się w pliku aplikacji `App/Web.config` , zostanie użyte połączenie.

* Jeśli baza danych nie istnieje, zostanie utworzona.

* Jeśli żadna z tabel z modelu nie istnieje w bazie danych, schemat dla bieżącego modelu zostanie dodany do bazy danych. Jeśli migracja jest włączona, są one używane do tworzenia bazy danych.

* Jeśli baza danych istnieje i EF6 wcześniej utworzyła schemat, schemat jest sprawdzany pod kątem zgodności z bieżącym modelem. Wyjątek jest zgłaszany, jeśli model został zmieniony od czasu utworzenia schematu.

**EF Core nie wykonuje żadnego z tych magicznych.**

* Połączenie z bazą danych musi być jawnie skonfigurowane w kodzie.

* Nie są wykonywane żadne inicjalizacje. Należy użyć `DbContext.Database.Migrate()` , aby zastosować migracje (lub `DbContext.Database.EnsureCreated()` i `EnsureDeleted()` utworzyć/usunąć bazę danych bez użycia migracji).

### <a name="code-first-table-naming-convention"></a>Konwencja nazewnictwa tabel Code First

EF6 uruchamia nazwę klasy jednostki za pomocą usługi pluralizacja, aby obliczyć domyślną nazwę tabeli, do której jednostka jest zamapowana.

EF Core używa nazwy `DbSet` właściwości, która jest widoczna dla jednostki w kontekście pochodnym. Jeśli jednostka nie ma `DbSet` właściwości, zostanie użyta nazwa klasy.
