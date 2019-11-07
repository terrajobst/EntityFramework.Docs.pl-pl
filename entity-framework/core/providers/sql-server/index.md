---
title: Dostawca bazy danych Microsoft SQL Server — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: dd352b81da05fa8ea8970495f20947bd109edf65
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655895"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="fdbae-102">Dostawca bazy danych EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="fdbae-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="fdbae-103">Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Microsoft SQL Server (w tym SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="fdbae-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="fdbae-104">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="fdbae-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="fdbae-105">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="fdbae-105">Install</span></span>

<span data-ttu-id="fdbae-106">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="fdbae-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="fdbae-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="fdbae-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="fdbae-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdbae-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="fdbae-109">Od wersji 3.0.0 dostawca odwołuje się do Microsoft. Data. SqlClient (poprzednie wersje zależały od system. Data. SqlClient).</span><span class="sxs-lookup"><span data-stu-id="fdbae-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="fdbae-110">Jeśli projekt przejmuje bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do poprawnego pakietu.</span><span class="sxs-lookup"><span data-stu-id="fdbae-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="fdbae-111">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="fdbae-111">Supported Database Engines</span></span>

* <span data-ttu-id="fdbae-112">Microsoft SQL Server (2012 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="fdbae-112">Microsoft SQL Server (2012 onwards)</span></span>
