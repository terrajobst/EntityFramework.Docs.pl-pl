---
title: "Testowanie przy użyciu programu Entity Framework - EF podstawowych składników"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a>Testowanie

Można przetestować składniki przy użyciu funkcji, która przybliża łączenia z bazą danych rzeczywistych, bez potrzeby rzeczywistej bazy danych operacji We/Wy.

Istnieją dwie główne opcje, aby to zrobić:
 * [Trybu w pamięci SQLite](sqlite.md) umożliwia pisanie wydajne testy względem dostawcy, który zachowuje się jak relacyjnej bazy danych.
 * [Dostawca InMemory](in-memory.md) jest lekki dostawcy, który ma minimalny zależności, ale nie zawsze zachowywać się jak relacyjnej bazy danych.
