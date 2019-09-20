---
title: Dostawca bazy danych Microsoft SQL Server — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149204"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="2dd79-102">Dostawca bazy danych EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="2dd79-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="2dd79-103">Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Microsoft SQL Server (w tym SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="2dd79-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="2dd79-104">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2dd79-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2dd79-105">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="2dd79-105">Install</span></span>

<span data-ttu-id="2dd79-106">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="2dd79-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="2dd79-107">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="2dd79-107">Supported Database Engines</span></span>

* <span data-ttu-id="2dd79-108">Microsoft SQL Server (2012 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="2dd79-108">Microsoft SQL Server (2012 onwards)</span></span>
