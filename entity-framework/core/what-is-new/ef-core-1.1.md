---
title: Co to jest nowa w programie EF 1.1 Core - EF Core
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054287"
---
# <a name="new-features-in-ef-core-11"></a>Nowe funkcje w wersji 1.1 Core EF

## <a name="modelling"></a>Modelowanie
### <a name="field-mapping"></a>Mapowanie pól
Umożliwia skonfigurowanie polem zapasowym dla właściwości. Może to być przydatne w przypadku właściwości tylko do odczytu lub dane, które zamiast właściwości metod Get/Set.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Mapowanie do tabel zoptymalizowanych pod kątem pamięci w programie SQL Server
Można określić, że jednostka jest zamapowana do tabeli jest zoptymalizowany pod kątem pamięci. Gdy przy użyciu EF Core można tworzyć i obsługiwać bazy danych na podstawie modelu (za pomocą migracji lub `Database.EnsureCreated()`), zostanie utworzona tabeli zoptymalizowanej pod kątem pamięci dla tych obiektów.

## <a name="change-tracking"></a>Śledzenie zmian
### <a name="additional-change-tracking-apis-from-ef6"></a>Śledzenie interfejsów API z EF6 dodatkowych zmian
Takie jak `Reload`, `GetModifiedProperties`, `GetDatabaseValues` itp.

## <a name="query"></a>Zapytanie
### <a name="explicit-loading"></a>Ładowanie jawne
Służy do wyzwalania wypełniania właściwości nawigacji jednostki, które zostało wcześniej załadowane z bazy danych.
### <a name="dbsetfind"></a>DbSet.Find
Zapewnia łatwy sposób pobrać oparte na jego wartości klucza podstawowego jednostki.

## <a name="other"></a>Inne
### <a name="connection-resiliency"></a>Elastyczność połączenia
Automatycznie ponownych prób nie polecenia bazy danych. Jest to szczególnie przydatne podczas połączenia SQL Azure, w których typowych błędów przejściowych.
### <a name="simplified-service-replacement"></a>Uproszczony wymienna
Ułatwia Zastąp wewnętrznych usług, które używa EF.
