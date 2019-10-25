---
title: Tworzenie i usuwanie interfejsów API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811787"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="89f9e-102">Tworzenie i upuszczanie interfejsów API</span><span class="sxs-lookup"><span data-stu-id="89f9e-102">Create and Drop APIs</span></span>

<span data-ttu-id="89f9e-103">Metody EnsureCreated i EnsureDeleted zapewniają uproszczoną alternatywę dla [migracji](migrations/index.md) w celu zarządzania schematem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="89f9e-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="89f9e-104">Te metody są przydatne w scenariuszach, gdy dane są przejściowe i mogą być porzucane, gdy zmienia się schemat.</span><span class="sxs-lookup"><span data-stu-id="89f9e-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="89f9e-105">Na przykład podczas tworzenia prototypów, w testach lub w lokalnych pamięciach podręcznych.</span><span class="sxs-lookup"><span data-stu-id="89f9e-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="89f9e-106">Niektórzy dostawcy (szczególnie nierelacyjne) nie obsługują migracji.</span><span class="sxs-lookup"><span data-stu-id="89f9e-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="89f9e-107">W przypadku tych dostawców EnsureCreated jest często najprostszym sposobem na zainicjowanie schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="89f9e-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="89f9e-108">EnsureCreated i migracje nie współpracują ze sobą.</span><span class="sxs-lookup"><span data-stu-id="89f9e-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="89f9e-109">W przypadku korzystania z migracji nie należy używać EnsureCreated, aby zainicjować schemat.</span><span class="sxs-lookup"><span data-stu-id="89f9e-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="89f9e-110">Przejście z EnsureCreated do migracji nie jest bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="89f9e-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="89f9e-111">Najprostszym sposobem, aby to zrobić, jest usunięcie bazy danych i jej ponowne utworzenie przy użyciu migracji.</span><span class="sxs-lookup"><span data-stu-id="89f9e-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="89f9e-112">Jeśli zamierzasz korzystać z migracji w przyszłości, najlepiej zacząć od migracji zamiast korzystać z EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="89f9e-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="89f9e-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="89f9e-113">EnsureDeleted</span></span>

<span data-ttu-id="89f9e-114">Metoda EnsureDeleted usunie bazę danych, jeśli istnieje.</span><span class="sxs-lookup"><span data-stu-id="89f9e-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="89f9e-115">Jeśli nie masz odpowiednich uprawnień, zostanie zgłoszony wyjątek.</span><span class="sxs-lookup"><span data-stu-id="89f9e-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="89f9e-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="89f9e-116">EnsureCreated</span></span>

<span data-ttu-id="89f9e-117">EnsureCreated utworzy bazę danych, jeśli nie istnieje, i zainicjuje schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="89f9e-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="89f9e-118">Jeśli istnieją tabele (w tym tabele dla innej klasy DbContext), schemat nie zostanie zainicjowany.</span><span class="sxs-lookup"><span data-stu-id="89f9e-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="89f9e-119">Wersje asynchroniczne tych metod są również dostępne.</span><span class="sxs-lookup"><span data-stu-id="89f9e-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="89f9e-120">Skrypt SQL</span><span class="sxs-lookup"><span data-stu-id="89f9e-120">SQL Script</span></span>

<span data-ttu-id="89f9e-121">Aby uzyskać kod SQL używany przez EnsureCreated, można użyć metody GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="89f9e-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="89f9e-122">Wiele klas DbContext</span><span class="sxs-lookup"><span data-stu-id="89f9e-122">Multiple DbContext classes</span></span>

<span data-ttu-id="89f9e-123">EnsureCreated działa tylko wtedy, gdy żadna tabela nie znajduje się w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="89f9e-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="89f9e-124">W razie potrzeby można napisać własne sprawdzenie, aby sprawdzić, czy schemat musi być zainicjowany, i użyć bazowej usługi IRelationalDatabaseCreator do zainicjowania schematu.</span><span class="sxs-lookup"><span data-stu-id="89f9e-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
