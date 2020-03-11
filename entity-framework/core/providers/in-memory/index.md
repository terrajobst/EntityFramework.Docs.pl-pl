---
title: Dostawca bazy danych inMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417220"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="f3f91-102">EF Core dostawcy bazy danych w pamięci</span><span class="sxs-lookup"><span data-stu-id="f3f91-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="f3f91-103">Ten dostawca bazy danych umożliwia Entity Framework Core używany z bazą danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f3f91-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="f3f91-104">Może to być przydatne do testowania, chociaż dostawca programu SQLite w trybie w pamięci może być bardziej odpowiednim zamiennikiem testu dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="f3f91-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="f3f91-105">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="f3f91-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="f3f91-106">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="f3f91-106">Install</span></span>

<span data-ttu-id="f3f91-107">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="f3f91-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="f3f91-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f3f91-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="f3f91-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3f91-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="f3f91-110">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="f3f91-110">Get Started</span></span>

<span data-ttu-id="f3f91-111">Poniższe zasoby pomogą Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="f3f91-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="f3f91-112">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="f3f91-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="f3f91-113">Testy przykładowej aplikacji UnicornStore</span><span class="sxs-lookup"><span data-stu-id="f3f91-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="f3f91-114">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="f3f91-114">Supported Database Engines</span></span>

<span data-ttu-id="f3f91-115">Baza danych pamięci w procesie (przeznaczona tylko do celów testowych)</span><span class="sxs-lookup"><span data-stu-id="f3f91-115">In-process memory database (designed for testing purposes only)</span></span>
