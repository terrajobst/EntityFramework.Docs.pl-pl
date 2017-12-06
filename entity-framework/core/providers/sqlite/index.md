---
title: "Dostawca bazy danych SQLite — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="13f4d-102">Dostawca bazy danych podstawowych EF SQLite</span><span class="sxs-lookup"><span data-stu-id="13f4d-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="13f4d-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="13f4d-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="13f4d-104">Dostawca jest przechowywany jako część [projektu EntityFramework GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="13f4d-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="13f4d-105">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="13f4d-105">Install</span></span>

<span data-ttu-id="13f4d-106">Zainstaluj [pakietu Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="13f4d-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="13f4d-107">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="13f4d-107">Get Started</span></span>

<span data-ttu-id="13f4d-108">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="13f4d-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="13f4d-109">Lokalne bazy danych SQLite na platformy uniwersalnej systemu Windows</span><span class="sxs-lookup"><span data-stu-id="13f4d-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="13f4d-110">Aplikacji .NET core do nowej bazy danych SQLite</span><span class="sxs-lookup"><span data-stu-id="13f4d-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="13f4d-111">Unicorn Clicker przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="13f4d-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="13f4d-112">Unicorn pakujący przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="13f4d-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="13f4d-113">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="13f4d-113">Supported Database Engines</span></span>

* <span data-ttu-id="13f4d-114">SQLite (3.7 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="13f4d-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="13f4d-115">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="13f4d-115">Supported Platforms</span></span>

* <span data-ttu-id="13f4d-116">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="13f4d-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="13f4d-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="13f4d-117">.NET Core</span></span>

* <span data-ttu-id="13f4d-118">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="13f4d-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="13f4d-119">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="13f4d-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="13f4d-120">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="13f4d-120">Limitations</span></span>

<span data-ttu-id="13f4d-121">Zobacz [ograniczenia SQLite](limitations.md) dla pewne ważne ograniczenia dostawcy bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="13f4d-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
