---
title: Co nowego w EF Core 1.1 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417518"
---
# <a name="new-features-in-ef-core-11"></a>Nowe funkcje w EF Core 1.1

## <a name="modeling"></a>Modelowanie

### <a name="field-mapping"></a>Mapowanie pola

Umożliwia skonfigurowanie pola zapasowego dla właściwości. Może to być przydatne dla właściwości tylko do odczytu lub danych, które mają Get/Set metody, a nie właściwości.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapowanie do tabel zoptymalizowanych pod kątem pamięci w programie SQL Server

Można określić, że tabela, do której jest mapowana encja, jest zoptymalizowana pod kątem pamięci. Podczas korzystania z EF Core do tworzenia i obsługi bazy danych `Database.EnsureCreated()`na podstawie modelu (z migracji lub), tabela zoptymalizowana pod kątem pamięci zostaną utworzone dla tych jednostek.

## <a name="change-tracking"></a>Śledzenie zmian

### <a name="additional-change-tracking-apis-from-ef6"></a>Dodatkowe interfejsy API śledzenia zmian z ef6

Takie `Reload`jak `GetModifiedProperties` `GetDatabaseValues` , , itp.

## <a name="query"></a>Zapytanie

### <a name="explicit-loading"></a>Jawne ładowanie

Umożliwia wyzwolenie zapełnienia właściwości nawigacji na encji, która została wcześniej załadowana z bazy danych.

### <a name="dbsetfind"></a>DbSet.Find (Znajdowanie zestawu DbSet.Find)

Zapewnia łatwy sposób pobierania jednostki na podstawie jego wartości klucza podstawowego.

## <a name="other"></a>Inne

### <a name="connection-resiliency"></a>Elastyczność połączenia

Automatycznie ponawia ponawia nieudane polecenia bazy danych. Jest to szczególnie przydatne podczas połączenia z platformą SQL Azure, gdzie błędy przejściowe są typowe.

### <a name="simplified-service-replacement"></a>Uproszczona wymiana usług

Ułatwia zastąpienie usług wewnętrznych, które ef używa.
