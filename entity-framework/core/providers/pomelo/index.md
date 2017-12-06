---
title: "Dostawca bazy danych MySQL pomelo — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="e7ceb-102">Dostawca pomelo EF podstawowej bazy danych dla programu MySQL</span><span class="sxs-lookup"><span data-stu-id="e7ceb-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="e7ceb-103">Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z programem MySQL.</span><span class="sxs-lookup"><span data-stu-id="e7ceb-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="e7ceb-104">Dostawca jest przechowywany jako część [projektu Foundation Pomelo](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="e7ceb-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="e7ceb-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e7ceb-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="e7ceb-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="e7ceb-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="e7ceb-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="e7ceb-107">Install</span></span>

<span data-ttu-id="e7ceb-108">Zainstaluj [pakietu Pomelo.EntityFrameworkCore.MySql NuGet](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="e7ceb-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="e7ceb-109">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="e7ceb-109">Get Started</span></span>

<span data-ttu-id="e7ceb-110">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="e7ceb-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="e7ceb-111">Pobieranie rozpoczęte dokumentacji</span><span class="sxs-lookup"><span data-stu-id="e7ceb-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="e7ceb-112">Yuuko Blog przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="e7ceb-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="e7ceb-113">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="e7ceb-113">Supported Database Engines</span></span>

* <span data-ttu-id="e7ceb-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="e7ceb-114">MySQL</span></span>
* <span data-ttu-id="e7ceb-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="e7ceb-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e7ceb-116">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="e7ceb-116">Supported Platforms</span></span>

* <span data-ttu-id="e7ceb-117">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="e7ceb-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="e7ceb-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7ceb-118">.NET Core</span></span>

* <span data-ttu-id="e7ceb-119">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="e7ceb-119">Mono (4.2.0 onwards)</span></span>
