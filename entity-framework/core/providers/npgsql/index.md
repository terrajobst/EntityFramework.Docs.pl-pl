---
title: Dostawca bazy danych Npgsql dla PostgreSQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="00468-102">Dostawca bazy danych podstawowych Npgsql EF, PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="00468-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="00468-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="00468-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="00468-104">Dostawca jest przechowywany jako część [projektu Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="00468-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="00468-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="00468-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="00468-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="00468-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="00468-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="00468-107">Install</span></span>

<span data-ttu-id="00468-108">Zainstaluj [pakietu Npgsql.EntityFrameworkCore.PostgreSQL NuGet](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="00468-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="00468-109">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="00468-109">Get Started</span></span>

<span data-ttu-id="00468-110">Zobacz [dokumentacji Npgsql](http://www.npgsql.org/efcore/index.html) rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="00468-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="00468-111">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="00468-111">Supported Database Engines</span></span>

* <span data-ttu-id="00468-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="00468-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="00468-113">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="00468-113">Supported Platforms</span></span>

* <span data-ttu-id="00468-114">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="00468-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="00468-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="00468-115">.NET Core</span></span>

* <span data-ttu-id="00468-116">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="00468-116">Mono (4.2.0 onwards)</span></span>
