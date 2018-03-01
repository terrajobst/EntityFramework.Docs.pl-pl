---
title: "Dostawca bazy danych programu Microsoft SQL Server Compact Edition — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="3bc82-102">Dostawca bazy danych programu Microsoft SQL Server Compact Edition EF Core</span><span class="sxs-lookup"><span data-stu-id="3bc82-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="3bc82-103">Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z programem SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="3bc82-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="3bc82-104">Dostawca jest przechowywany jako część [ErikEJ/EntityFramework.SqlServerCompact GitHub projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="3bc82-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="3bc82-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3bc82-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="3bc82-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="3bc82-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="3bc82-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="3bc82-107">Install</span></span>

<span data-ttu-id="3bc82-108">Aby pracować z programu SQL Server Compact Edition 4.0, zainstaluj [pakietu EntityFrameworkCore.SqlServerCompact40 NuGet](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="3bc82-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="3bc82-109">Aby pracować z programu SQL Server Compact Edition 3.5, zainstaluj [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="3bc82-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="3bc82-110">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="3bc82-110">Get Started</span></span>

<span data-ttu-id="3bc82-111">Zobacz [wprowadzenie dokumentacji w witrynie projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="3bc82-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="3bc82-112">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="3bc82-112">Supported Database Engines</span></span>

* <span data-ttu-id="3bc82-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="3bc82-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="3bc82-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="3bc82-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3bc82-115">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="3bc82-115">Supported Platforms</span></span>

* <span data-ttu-id="3bc82-116">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="3bc82-116">.NET Framework (4.5.1 onwards)</span></span>
