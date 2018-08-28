---
title: Bazy danych SQLite dostawcy - ograniczenia — EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 69c40fcd8b7ddb925728b1bad9992ad2a81e7540
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994667"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Ograniczenia dotyczące dostawcy bazy danych SQLite EF Core

Dostawca bazy danych SQLite ma kilka ograniczeń migracji. Większość z tych ograniczeń są wynikiem ograniczenia podstawowego aparatu bazy danych SQLite i nie są specyficzne dla platformy EF.

## <a name="modeling-limitations"></a>Ograniczenia modelowania

Wspólne biblioteki relacyjnych (udostępnione przez dostawców relacyjnej bazy danych programu Entity Framework) definiuje interfejsów API do modelowania pojęcia, które są wspólne dla większości relacyjnych baz danych. Kilka z tych koncepcji nie są obsługiwane przez dostawcę bazy danych SQLite.

* Schematy
* Sekwencje

## <a name="migrations-limitations"></a>Ograniczenia migracji

Aparat bazy danych SQLite nie obsługuje szereg operacji schematu, które są obsługiwane przez większość innych relacyjnych baz danych. Jeśli spróbujesz zastosować jedną nieobsługiwane operacje do bazy danych SQLite, a następnie `NotSupportedException` zostanie zgłoszony.

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
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (pusta)  | 2.0              |
| DropSchema           | ✔ (pusta)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Aktualizacja               | ✔          | 2.0              |
| Usuwanie               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Obejście ograniczenia dotyczące migracji

Można obejść niektóre z tych ograniczeń, ręcznie wpisując kod w migracji do wykonania tabeli ponownie skompilować. Odbuduj tabelę obejmuje zmianę nazwy istniejącej tabeli, tworzenie nowej tabeli, kopiowanie danych do nowej tabeli i usunięcie starych tabeli. Należy użyć `Sql(string)` metodę, aby wykonać niektóre z tych kroków.

Zobacz [co inne rodzaje z zmiany schematu tabeli](http://sqlite.org/lang_altertable.html#otheralter) w dokumentacji oprogramowania SQLite, aby uzyskać więcej informacji.

W przyszłości EF mogą obsługiwać kilku z tych operacji przy użyciu podejścia Odbuduj tabelę w sposób niewidoczny. Możesz [śledzenie tej funkcji w projekcie usługi GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
