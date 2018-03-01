---
title: "Bazy danych SQLite dostawcy — ograniczenia - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 3e0f375fa3e01747565cc158af02f6d21f6ae898
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Ograniczenia dotyczące dostawcy bazy danych SQLite EF Core

Dostawca bazy danych SQLite ma różne ograniczenia migracji. Większość tych ograniczeń wyniku ograniczenia w podstawowej bazy danych SQLite i nie są specyficzne dla EF.

## <a name="modeling-limitations"></a>Ograniczenia modelowania

Wspólna biblioteka relacyjnych (udostępniony przez dostawców relacyjnej bazy danych programu Entity Framework) definiuje interfejsów API do modelowania pojęcia, które są wspólne dla większości relacyjnych baz danych. Kilka tych pojęć nie są obsługiwane przez dostawcę danych SQLite.

* Schematy
* Sekwencje

## <a name="migrations-limitations"></a>Ograniczenia migracji

Aparat bazy danych SQLite nie obsługuje wielu operacji schematu, które są obsługiwane przez większość innych relacyjnych baz danych. Jeśli spróbujesz dotyczą jednej z nieobsługiwanej operacji bazy danych SQLite, a następnie `NotSupportedException` zostanie wygenerowany.

| Operacja            | Obsługiwane? | Wymaga wersji |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.1              |
| RenameIndex          | ✔          | 1.0              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (pusta)  | 2.0              |
| DropSchema           | ✔ (pusta)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Aktualizacja               | ✔          | 2.0              |
| Usuń               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Obejście ograniczenia dotyczące migracji

Niektóre można obejść tych ograniczeń ręcznie pisanie kodu w migracji do wykonania tabeli odbudować. Odbuduj tabelę obejmuje zmianę nazwy istniejącego tabeli, tworzy nową tabelę kopiowanie danych do nowej tabeli i usunięcie starej tabeli. Będą musieli używać `Sql(string)` metody do wykonania niektórych z tych kroków.

Zobacz [co inne rodzaje z zmiany schematu tabeli](http://sqlite.org/lang_altertable.html#otheralter) w dokumentacji SQLite, aby uzyskać więcej informacji.

W przyszłości EF może obsługiwać niektóre z tych operacji przy użyciu podejścia Odbuduj tabelę w obszarze obejmuje. Możesz [śledzenie tej funkcji w naszym projektu GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
