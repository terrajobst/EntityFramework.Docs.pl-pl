---
title: Dostawca bazy danych SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994005"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="056e3-102">Dostawca bazy danych systemu SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="056e3-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="056e3-103">Ten dostawca bazy danych umożliwia platformy Entity Framework Core do użycia z bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="056e3-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="056e3-104">Dostawca jest przechowywane jako część [projektu platformy Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="056e3-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="056e3-105">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="056e3-105">Install</span></span>

<span data-ttu-id="056e3-106">Zainstaluj [pakietu Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="056e3-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="056e3-107">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="056e3-107">Get Started</span></span>

<span data-ttu-id="056e3-108">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="056e3-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="056e3-109">Lokalne bazy danych SQLite na platformy uniwersalnej systemu Windows</span><span class="sxs-lookup"><span data-stu-id="056e3-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="056e3-110">Aplikacji .NET core do nowej bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="056e3-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="056e3-111">Unicorn Clicker przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="056e3-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="056e3-112">Unicorn Packer przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="056e3-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="056e3-113">Obsługiwane baz danych</span><span class="sxs-lookup"><span data-stu-id="056e3-113">Supported Database Engines</span></span>

* <span data-ttu-id="056e3-114">Bazy danych SQLite (3.7 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="056e3-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="056e3-115">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="056e3-115">Supported Platforms</span></span>

* <span data-ttu-id="056e3-116">.NET framework (4.5.1 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="056e3-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="056e3-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="056e3-117">.NET Core</span></span>

* <span data-ttu-id="056e3-118">Narzędzie mono (4.2.0 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="056e3-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="056e3-119">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="056e3-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="056e3-120">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="056e3-120">Limitations</span></span>

<span data-ttu-id="056e3-121">Zobacz [ograniczenia SQLite](limitations.md) dla pewne ważne ograniczenia dotyczące dostawcy bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="056e3-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
