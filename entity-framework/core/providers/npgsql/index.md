---
title: Dostawca bazy danych Npgsql dla PostgreSQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="4de49-102">Dostawca bazy danych podstawowych Npgsql EF, PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4de49-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="4de49-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="4de49-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="4de49-104">Dostawca jest przechowywany jako część [projektu Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="4de49-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="4de49-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4de49-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="4de49-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="4de49-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="4de49-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="4de49-107">Install</span></span>

<span data-ttu-id="4de49-108">Zainstaluj [pakietu Npgsql.EntityFrameworkCore.PostgreSQL NuGet](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="4de49-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="4de49-109">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="4de49-109">Get Started</span></span>

<span data-ttu-id="4de49-110">Zobacz [dokumentacji Npgsql](http://www.npgsql.org/efcore/index.html) rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="4de49-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="4de49-111">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="4de49-111">Supported Database Engines</span></span>

* <span data-ttu-id="4de49-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4de49-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4de49-113">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="4de49-113">Supported Platforms</span></span>

* <span data-ttu-id="4de49-114">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="4de49-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="4de49-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="4de49-115">.NET Core</span></span>

* <span data-ttu-id="4de49-116">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="4de49-116">Mono (4.2.0 onwards)</span></span>
