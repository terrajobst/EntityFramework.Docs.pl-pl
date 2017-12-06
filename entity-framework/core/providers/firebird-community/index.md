---
title: Dostawca bazy danych FirebirdSQL - EF Core
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="cac86-102">Dostawca bazy danych podstawowych EF firebird</span><span class="sxs-lookup"><span data-stu-id="cac86-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="cac86-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="cac86-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="cac86-104">Dostawca jest przechowywany jako część [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL projektu GitHub](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="cac86-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="cac86-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cac86-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="cac86-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="cac86-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="cac86-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="cac86-107">Install</span></span>

<span data-ttu-id="cac86-108">Zainstaluj [pakietu EntityFrameworkCore.FirebirdSQL NuGet](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="cac86-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="cac86-109">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="cac86-109">Get Started</span></span>

<span data-ttu-id="cac86-110">Zobacz [wprowadzenie dokumentacji w witrynie projektu](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="cac86-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="cac86-111">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="cac86-111">Supported Database Engines</span></span>

* <span data-ttu-id="cac86-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="cac86-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="cac86-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="cac86-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="cac86-114">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="cac86-114">Supported Platforms</span></span>

* <span data-ttu-id="cac86-115">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="cac86-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="cac86-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="cac86-116">.NET Core</span></span>

* <span data-ttu-id="cac86-117">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="cac86-117">Mono (4.2.0 onwards)</span></span>
