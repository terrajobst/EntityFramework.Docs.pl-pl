---
title: Tworzenie i upuszczanie interfejsów API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285642"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="f1be0-102">Tworzenie i upuszczanie interfejsów API</span><span class="sxs-lookup"><span data-stu-id="f1be0-102">Create and Drop APIs</span></span>

<span data-ttu-id="f1be0-103">Metody EnsureCreated i EnsureDeleted udostępniają uproszczone zamiast [migracje](migrations/index.md) zarządzania schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1be0-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="f1be0-104">Może to być przydatne w sytuacjach, gdy dane są przejściowy i można było porzucić po zmianie schematu.</span><span class="sxs-lookup"><span data-stu-id="f1be0-104">This is useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="f1be0-105">Na przykład podczas tworzenia prototypów w testach lub pamięciach podręcznych.</span><span class="sxs-lookup"><span data-stu-id="f1be0-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="f1be0-106">Niektórzy dostawcy (zwłaszcza tymi nierelacyjnych) nie obsługuje migracji.</span><span class="sxs-lookup"><span data-stu-id="f1be0-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="f1be0-107">W tym przypadku EnsureCreated często jest najprostszym sposobem zainicjować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1be0-107">For these, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="f1be0-108">EnsureCreated i migracje nie działają dobrze ze sobą.</span><span class="sxs-lookup"><span data-stu-id="f1be0-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="f1be0-109">Jeśli używasz migracji, nie należy używać EnsureCreated zainicjować schematu.</span><span class="sxs-lookup"><span data-stu-id="f1be0-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="f1be0-110">Przechodzenie ze EnsureCreated do migracji nie jest bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="f1be0-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="f1be0-111">Jest simpelest sposób osiągnięcia tego celu porzucenia bazy danych i ponownego utworzenia go za pomocą migracji.</span><span class="sxs-lookup"><span data-stu-id="f1be0-111">The simpelest way to achieve this is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="f1be0-112">Jeśli przewidujesz używanie migracji w przyszłości, najlepiej zamiast EnsureCreated, po prostu zacznij z migracji.</span><span class="sxs-lookup"><span data-stu-id="f1be0-112">If you anticipate using Migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="f1be0-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="f1be0-113">EnsureDeleted</span></span>

<span data-ttu-id="f1be0-114">Metoda EnsureDeleted spowoduje porzucenie bazy danych, jeśli taki istnieje.</span><span class="sxs-lookup"><span data-stu-id="f1be0-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="f1be0-115">Wyjątek jest generowany, jeśli nie masz odpowiednie uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="f1be0-115">If you don't have the appropiate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="f1be0-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="f1be0-116">EnsureCreated</span></span>

<span data-ttu-id="f1be0-117">EnsureCreated spowoduje utworzenie bazy danych, jeśli nie istnieje i zainicjować schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1be0-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="f1be0-118">Jeśli istnieje żadnych tabel (w tym tabel dla innej klasy DbContext) schematu nie można zainicjować.</span><span class="sxs-lookup"><span data-stu-id="f1be0-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="f1be0-119">Ponadto dostępnych asynchronicznych wersji tych metod.</span><span class="sxs-lookup"><span data-stu-id="f1be0-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="f1be0-120">Skrypt SQL</span><span class="sxs-lookup"><span data-stu-id="f1be0-120">SQL Script</span></span>

<span data-ttu-id="f1be0-121">Aby uzyskać SQL używane przez EnsureCreated, można użyć metody GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="f1be0-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="f1be0-122">Wiele klas typu DbContext</span><span class="sxs-lookup"><span data-stu-id="f1be0-122">Multiple DbContext classes</span></span>

<span data-ttu-id="f1be0-123">EnsureCreated działa tylko w przypadku, gdy żadna tabela znajdują się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f1be0-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="f1be0-124">Jeśli to konieczne, można napisać własne sprawdzenie, czy schemat musi zostać zainicjowany i usługa bazowego IRelationalDatabaseCreator zainicjować schematu.</span><span class="sxs-lookup"><span data-stu-id="f1be0-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
