---
title: "Dostawca bazy danych MySQL — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="c9744-102">Dostawca bazy danych programu MySQL EF Core</span><span class="sxs-lookup"><span data-stu-id="c9744-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="c9744-103">Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z programem MySQL.</span><span class="sxs-lookup"><span data-stu-id="c9744-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="c9744-104">Dostawca jest przechowywany jako część [projektu MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="c9744-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="c9744-105">Ten dostawca jest wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="c9744-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="c9744-106">Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c9744-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c9744-107">Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.</span><span class="sxs-lookup"><span data-stu-id="c9744-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c9744-108">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="c9744-108">Install</span></span>

<span data-ttu-id="c9744-109">Zainstaluj [pakietu MySql.Data.EntityFrameworkCore NuGet](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="c9744-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="c9744-110">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="c9744-110">Get Started</span></span>

<span data-ttu-id="c9744-111">Zobacz [począwszy od dostawcy MySQL EF Core i Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="c9744-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c9744-112">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="c9744-112">Supported Database Engines</span></span>

* <span data-ttu-id="c9744-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="c9744-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c9744-114">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="c9744-114">Supported Platforms</span></span>

* <span data-ttu-id="c9744-115">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="c9744-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="c9744-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9744-116">.NET Core</span></span>

<span data-ttu-id="c9744-117">Pamiętaj zapoznać się z dokumentacją MySQL, aby uzyskać informacje dotyczące zgodności wersji [tutaj](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) i [tutaj](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="c9744-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
