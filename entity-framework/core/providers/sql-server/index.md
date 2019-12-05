---
title: Dostawca bazy danych Microsoft SQL Server — EF Core
description: Dokumentacja dostawcy bazy danych, która umożliwia Entity Framework Core, które mają być używane z Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 18a69789ff4ae013c1d60bb6d34ca5c27ee285c2
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824774"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="2800d-103">Dostawca bazy danych EF Core Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="2800d-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="2800d-104">Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Microsoft SQL Server (w tym Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="2800d-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="2800d-105">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2800d-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2800d-106">Instalacja programu</span><span class="sxs-lookup"><span data-stu-id="2800d-106">Install</span></span>

<span data-ttu-id="2800d-107">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="2800d-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="2800d-108">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2800d-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="2800d-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2800d-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="2800d-110">Od wersji 3.0.0 dostawca odwołuje się do Microsoft. Data. SqlClient (poprzednie wersje zależały od system. Data. SqlClient).</span><span class="sxs-lookup"><span data-stu-id="2800d-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="2800d-111">Jeśli projekt pobiera bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do pakietu Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2800d-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="2800d-112">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="2800d-112">Supported Database Engines</span></span>

* <span data-ttu-id="2800d-113">Microsoft SQL Server (2012 lub nowsza)</span><span class="sxs-lookup"><span data-stu-id="2800d-113">Microsoft SQL Server (2012 onwards)</span></span>
