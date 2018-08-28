---
title: Testowanie składników używający narzędzia Entity Framework - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997840"
---
# <a name="testing"></a>Testowanie

Można przetestować składników za pomocą coś, co przybliża z bazą danych rzeczywistych, bez konieczności operacji We/Wy bazy danych.

Istnieją dwie główne opcje w ten sposób:
 * [Tryb w pamięci SQLite](sqlite.md) pozwala na zapis efektywne testy względem dostawcy, który zachowuje się jak relacyjnej bazy danych.
 * [Dostawca InMemory](in-memory.md) jest uproszczone dostawcę, który ma minimalne zależności, ale nie zawsze zachowują się jak relacyjnej bazy danych.
