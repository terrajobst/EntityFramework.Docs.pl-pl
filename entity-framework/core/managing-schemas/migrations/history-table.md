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
# <a name="custom-migrations-history-table"></a><span data-ttu-id="9a47a-102">Tabela historii niestandardowych migracji</span><span class="sxs-lookup"><span data-stu-id="9a47a-102">Custom Migrations History Table</span></span>

<span data-ttu-id="9a47a-103">Domyślnie program EF Core śledzi, które migracje zostały zastosowane do bazy danych, rejestrując je w tabeli o nazwie `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="9a47a-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="9a47a-104">Z różnych powodów możesz chcieć dostosować tę tabelę, aby lepiej odpowiadała Twoim potrzebom.</span><span class="sxs-lookup"><span data-stu-id="9a47a-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9a47a-105">Jeśli dostosowano tabelę historii migracji *po* zastosowaniu migracji, użytkownik jest odpowiedzialny za aktualizowanie istniejącej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9a47a-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="9a47a-106">Nazwa schematu i tabeli</span><span class="sxs-lookup"><span data-stu-id="9a47a-106">Schema and table name</span></span>

<span data-ttu-id="9a47a-107">Można zmienić nazwę schematu i tabeli przy użyciu metody `MigrationsHistoryTable()` w `OnConfiguring()` (lub `ConfigureServices()` na ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="9a47a-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="9a47a-108">Oto przykład użycia dostawcy EF Core SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9a47a-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="9a47a-109">Inne zmiany</span><span class="sxs-lookup"><span data-stu-id="9a47a-109">Other changes</span></span>

<span data-ttu-id="9a47a-110">Aby skonfigurować dodatkowe aspekty tabeli, Przesłoń i Zastąp `IHistoryRepository` usługę specyficzną dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="9a47a-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="9a47a-111">Oto przykład zmiany nazwy kolumny MigrationId na *Identyfikator* na SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9a47a-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="9a47a-112">`SqlServerHistoryRepository` znajduje się wewnątrz wewnętrznej przestrzeni nazw i może ulec zmianie w przyszłych wersjach.</span><span class="sxs-lookup"><span data-stu-id="9a47a-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
