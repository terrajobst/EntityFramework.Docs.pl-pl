---
title: Tworzenie i upuszczanie interfejsów API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688632"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="65feb-102">Tworzenie i upuszczanie interfejsów API</span><span class="sxs-lookup"><span data-stu-id="65feb-102">Create and Drop APIs</span></span>

<span data-ttu-id="65feb-103">Metody EnsureCreated i EnsureDeleted udostępniają uproszczone zamiast [migracje](migrations/index.md) zarządzania schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="65feb-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="65feb-104">Te metody są przydatne w scenariuszach danych jest przejściowy i można było porzucić po zmianie schematu.</span><span class="sxs-lookup"><span data-stu-id="65feb-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="65feb-105">Na przykład podczas tworzenia prototypów w testach lub pamięciach podręcznych.</span><span class="sxs-lookup"><span data-stu-id="65feb-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="65feb-106">Niektórzy dostawcy (zwłaszcza tymi nierelacyjnych) nie obsługuje migracji.</span><span class="sxs-lookup"><span data-stu-id="65feb-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="65feb-107">W przypadku tych dostawców EnsureCreated często jest najprostszym sposobem zainicjować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="65feb-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="65feb-108">EnsureCreated i migracje nie działają dobrze ze sobą.</span><span class="sxs-lookup"><span data-stu-id="65feb-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="65feb-109">Jeśli używasz migracji, nie należy używać EnsureCreated zainicjować schematu.</span><span class="sxs-lookup"><span data-stu-id="65feb-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="65feb-110">Przechodzenie ze EnsureCreated do migracji nie jest bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="65feb-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="65feb-111">Najprostszym sposobem, aby to zrobił jest porzucenia bazy danych i ponownego utworzenia go za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="65feb-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="65feb-112">Jeśli przewidujesz używanie migracji w przyszłości, najlepiej zamiast EnsureCreated, po prostu zacznij z migracji.</span><span class="sxs-lookup"><span data-stu-id="65feb-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="65feb-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="65feb-113">EnsureDeleted</span></span>

<span data-ttu-id="65feb-114">Metoda EnsureDeleted spowoduje porzucenie bazy danych, jeśli taki istnieje.</span><span class="sxs-lookup"><span data-stu-id="65feb-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="65feb-115">Jeśli nie masz odpowiednich uprawnień, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="65feb-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="65feb-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="65feb-116">EnsureCreated</span></span>

<span data-ttu-id="65feb-117">EnsureCreated spowoduje utworzenie bazy danych, jeśli nie istnieje i zainicjować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="65feb-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="65feb-118">Jeśli istnieje żadnych tabel (w tym tabel dla innej klasy DbContext) schematu nie można zainicjować.</span><span class="sxs-lookup"><span data-stu-id="65feb-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="65feb-119">Ponadto dostępnych asynchronicznych wersji tych metod.</span><span class="sxs-lookup"><span data-stu-id="65feb-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="65feb-120">Skrypt SQL</span><span class="sxs-lookup"><span data-stu-id="65feb-120">SQL Script</span></span>

<span data-ttu-id="65feb-121">Aby uzyskać SQL używane przez EnsureCreated, można użyć metody GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="65feb-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="65feb-122">Wiele klas typu DbContext</span><span class="sxs-lookup"><span data-stu-id="65feb-122">Multiple DbContext classes</span></span>

<span data-ttu-id="65feb-123">EnsureCreated działa tylko w przypadku, gdy żadna tabela znajdują się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="65feb-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="65feb-124">Jeśli to konieczne, można napisać własne sprawdzenie, czy schemat musi zostać zainicjowany i usługa bazowego IRelationalDatabaseCreator zainicjować schematu.</span><span class="sxs-lookup"><span data-stu-id="65feb-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
