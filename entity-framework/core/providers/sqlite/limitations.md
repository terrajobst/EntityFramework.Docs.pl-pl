---
title: "Bazy danych SQLite dostawcy — ograniczenia - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Ograniczenia dotyczące dostawcy bazy danych SQLite EF Core

Dostawca bazy danych SQLite ma różne ograniczenia migracji. Większość tych ograniczeń wyniku ograniczenia w podstawowej bazy danych SQLite i nie są specyficzne dla EF.

## <a name="modeling-limitations"></a>Ograniczenia modelowania

Wspólna biblioteka relacyjnych (udostępniony przez dostawców relacyjnej bazy danych programu Entity Framework) definiuje interfejsów API do modelowania pojęcia, które są wspólne dla większości relacyjnych baz danych. Kilka tych pojęć nie są obsługiwane przez dostawcę danych SQLite.

* Schematy
* Sekwencje

## <a name="migrations-limitations"></a>Ograniczenia migracji

Aparat bazy danych SQLite nie obsługuje wielu operacji schematu, które są obsługiwane przez większość innych relacyjnych baz danych. Jeśli spróbujesz dotyczą jednej z nieobsługiwanej operacji bazy danych SQLite, a następnie `NotSupportedException` zostanie wygenerowany.

| Operacja            | Obsługiwane? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| AddForeignKey        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| CreateTable          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DropIndex            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| RenameColumn         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>Obejście ograniczenia dotyczące migracji

Niektóre można obejść tych ograniczeń ręcznie pisanie kodu w migracji do wykonania tabeli odbudować. Odbuduj tabelę obejmuje zmianę nazwy istniejącego tabeli, tworzy nową tabelę kopiowanie danych do nowej tabeli i usunięcie starej tabeli. Będą musieli używać `Sql(string)` metody do wykonania niektórych z tych kroków.

Zobacz [co inne rodzaje z zmiany schematu tabeli](http://sqlite.org/lang_altertable.html#otheralter) w dokumentacji SQLite, aby uzyskać więcej informacji.

W przyszłości EF może obsługiwać niektóre z tych operacji przy użyciu podejścia Odbuduj tabelę w obszarze obejmuje. Możesz [śledzenie tej funkcji w naszym projektu GitHub](https://github.com/aspnet/EntityFramework/issues/329).
