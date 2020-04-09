---
title: Dostawca bazy danych InMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417220"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="6033f-102">Dostawca baz danych EF Core w pamięci</span><span class="sxs-lookup"><span data-stu-id="6033f-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="6033f-103">Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="6033f-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="6033f-104">Może to być przydatne do testowania, chociaż dostawca SQLite w trybie w pamięci może być bardziej odpowiednie zastąpienie testu dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="6033f-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="6033f-105">Dostawca jest utrzymywany w ramach [projektu core entity framework](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="6033f-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="6033f-106">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="6033f-106">Install</span></span>

<span data-ttu-id="6033f-107">Zainstaluj [pakiet Microsoft.EntityFrameWorkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="6033f-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="6033f-108">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="6033f-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="6033f-109">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6033f-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="6033f-110">Rozpoczęcie pracy</span><span class="sxs-lookup"><span data-stu-id="6033f-110">Get Started</span></span>

<span data-ttu-id="6033f-111">Poniższe zasoby pomogą Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="6033f-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="6033f-112">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="6033f-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="6033f-113">Testy przykładowych aplikacji UnicornStore</span><span class="sxs-lookup"><span data-stu-id="6033f-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="6033f-114">Obsługiwane aparaty baz danych</span><span class="sxs-lookup"><span data-stu-id="6033f-114">Supported Database Engines</span></span>

<span data-ttu-id="6033f-115">Baza danych pamięci w trakcie procesu (przeznaczona wyłącznie do celów testowych)</span><span class="sxs-lookup"><span data-stu-id="6033f-115">In-process memory database (designed for testing purposes only)</span></span>
