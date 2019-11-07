---
title: Tabela historii niestandardowych migracji — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655708"
---
# <a name="custom-migrations-history-table"></a>Tabela historii niestandardowych migracji

Domyślnie program EF Core śledzi, które migracje zostały zastosowane do bazy danych, rejestrując je w tabeli o nazwie `__EFMigrationsHistory`. Z różnych powodów możesz chcieć dostosować tę tabelę, aby lepiej odpowiadała Twoim potrzebom.

> [!IMPORTANT]
> Jeśli dostosowano tabelę historii migracji *po* zastosowaniu migracji, użytkownik jest odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.

## <a name="schema-and-table-name"></a>Nazwa schematu i tabeli

Można zmienić nazwę schematu i tabeli przy użyciu metody `MigrationsHistoryTable()` w `OnConfiguring()` (lub `ConfigureServices()` na ASP.NET Core). Oto przykład użycia dostawcy EF Core SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Inne zmiany

Aby skonfigurować dodatkowe aspekty tabeli, Przesłoń i Zastąp `IHistoryRepository` usługę specyficzną dla dostawcy. Oto przykład zmiany nazwy kolumny MigrationId na *Identyfikator* na SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` znajduje się wewnątrz wewnętrznej przestrzeni nazw i może ulec zmianie w przyszłych wersjach.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
