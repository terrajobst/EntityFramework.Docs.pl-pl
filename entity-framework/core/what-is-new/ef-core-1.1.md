---
title: What's new in EF Core 1.1 — EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995438"
---
# <a name="new-features-in-ef-core-11"></a>Nowe funkcje programu EF Core 1.1

## <a name="modelling"></a>Modelowanie
### <a name="field-mapping"></a>Mapowanie pól
Umożliwia skonfigurowanie z polem zapasowym dla właściwości. Może to być przydatne w przypadku właściwości tylko do odczytu lub danymi, które mają metod Get/Set, a nie właściwości.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapowanie do tabel zoptymalizowanych pod kątem pamięci w programie SQL Server
Można określić, czy jednostka jest zamapowana do tabel jest zoptymalizowane pod kątem pamięci. Podczas tworzenia i bazę danych przy użyciu programu EF Core na podstawie modelu (przy użyciu migracji lub `Database.EnsureCreated()`), zoptymalizowana pod kątem pamięci tabeli zostanie utworzony dla tych jednostek.

## <a name="change-tracking"></a>Śledzenie zmian
### <a name="additional-change-tracking-apis-from-ef6"></a>Kolejna zmiana, śledzenie interfejsów API z programu EF6
Takie jak `Reload`, `GetModifiedProperties`, `GetDatabaseValues` itp.

## <a name="query"></a>Zapytanie
### <a name="explicit-loading"></a>Jawne ładowanie
Służy do wyzwalania populacji właściwości nawigacji jednostki, która została wcześniej załadowana z bazy danych.
### <a name="dbsetfind"></a>DbSet.Find
Zapewnia łatwy sposób pobrać jednostki, w oparciu o jego wartość klucza podstawowego.

## <a name="other"></a>Inne
### <a name="connection-resiliency"></a>Elastyczność połączenia
Automatycznie ponownych prób nie powiodło się polecenia bazy danych. Jest to szczególnie przydatne podczas połączenia z platformą Azure SQL, w których typowych błędów przejściowych.
### <a name="simplified-service-replacement"></a>Uproszczone wymienna
Ułatwia Zastąp wewnętrznych usług, których używa EF.
