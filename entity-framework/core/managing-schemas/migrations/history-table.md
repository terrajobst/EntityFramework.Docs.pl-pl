---
title: Tabela historii niestandardowych migracji — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416844"
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
