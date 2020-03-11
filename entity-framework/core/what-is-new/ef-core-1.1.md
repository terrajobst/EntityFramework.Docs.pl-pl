---
title: Co nowego w EF Core 1,1 — EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417518"
---
# <a name="new-features-in-ef-core-11"></a>Nowe funkcje w EF Core 1,1

## <a name="modeling"></a>Modelowanie

### <a name="field-mapping"></a>Mapowanie pól

Umożliwia skonfigurowanie pola zapasowego dla właściwości. Może to być przydatne w przypadku właściwości tylko do odczytu lub danych, które mają metody get/set zamiast właściwości.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapowanie do tabel zoptymalizowanych pod kątem pamięci w SQL Server

Można określić, że tabela, do której jest zamapowana jednostka, jest zoptymalizowana pod kątem pamięci. W przypadku korzystania z EF Core do tworzenia i konserwowania bazy danych na podstawie modelu (z migracją lub `Database.EnsureCreated()`) dla tych jednostek zostanie utworzona tabela zoptymalizowana pod kątem pamięci.

## <a name="change-tracking"></a>Śledzenie zmian

### <a name="additional-change-tracking-apis-from-ef6"></a>Dodatkowe interfejsy API śledzenia zmian z EF6

Takie jak `Reload`, `GetModifiedProperties`, `GetDatabaseValues` itd.

## <a name="query"></a>Zapytanie

### <a name="explicit-loading"></a>Jawne ładowanie

Umożliwia wyzwalanie wypełniania właściwości nawigacji w jednostce, która została wcześniej załadowana z bazy danych.

### <a name="dbsetfind"></a>Nieogólnymi. Find

Zapewnia łatwy sposób pobierania jednostki na podstawie jej wartości klucza podstawowego.

## <a name="other"></a>Inne

### <a name="connection-resiliency"></a>Elastyczność połączenia

Automatyczne ponawianie próby nieudanych prób wykonania bazy danych. Jest to szczególnie przydatne w przypadku połączeń z usługą SQL Azure, w przypadku których błędy przejściowe są wspólne.

### <a name="simplified-service-replacement"></a>Uproszczone zastępowanie usług

Ułatwia zastępowanie usług wewnętrznych używanych przez program Dr.
