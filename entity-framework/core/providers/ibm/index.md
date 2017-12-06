---
title: "Dostawca danych IBM do bazy danych serwera (bazy danych DB2) — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="82316-102">IBM danych serwera (bazy danych DB2) EF podstawowej bazy danych dostawców</span><span class="sxs-lookup"><span data-stu-id="82316-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="82316-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z IBM danych serwera.</span><span class="sxs-lookup"><span data-stu-id="82316-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="82316-104">Dostawca jest obsługiwana przez IBM.</span><span class="sxs-lookup"><span data-stu-id="82316-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="82316-105">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="82316-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="82316-106">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="82316-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="82316-107">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="82316-107">Install</span></span>

<span data-ttu-id="82316-108">Aby pracować z IBM danych serwera w systemie Windows, należy zainstalować [IBM. Pakiet EntityFrameworkCore NuGet](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="82316-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="82316-109">Do pracy z IBM danych serwera w systemie Linux należy zainstalować [IBM. Pakiet NuGet EntityFrameworkCore lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="82316-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="82316-110">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="82316-110">Get Started</span></span>

[<span data-ttu-id="82316-111">Wprowadzenie do korzystania z dostawcy .NET IBM dla platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="82316-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="82316-112">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="82316-112">Supported Database Engines</span></span>

* <span data-ttu-id="82316-113">zOS</span><span class="sxs-lookup"><span data-stu-id="82316-113">zOS</span></span>
* <span data-ttu-id="82316-114">W tym IBM dashDB LUW</span><span class="sxs-lookup"><span data-stu-id="82316-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="82316-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="82316-115">IBM I</span></span>
* <span data-ttu-id="82316-116">Programu Informix</span><span class="sxs-lookup"><span data-stu-id="82316-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="82316-117">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="82316-117">Supported Platforms</span></span>

* <span data-ttu-id="82316-118">.NET framework (4.6 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="82316-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="82316-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="82316-119">.NET Core</span></span>
