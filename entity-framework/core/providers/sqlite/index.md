---
title: Dostawca bazy danych programu SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417778"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="2d9fb-102">Dostawca bazy danych EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="2d9fb-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="2d9fb-103">Ten dostawca bazy danych umożliwia Entity Framework Core używany z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="2d9fb-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="2d9fb-104">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2d9fb-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2d9fb-105">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="2d9fb-105">Install</span></span>

<span data-ttu-id="2d9fb-106">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="2d9fb-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2d9fb-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2d9fb-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="2d9fb-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d9fb-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="2d9fb-109">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="2d9fb-109">Supported Database Engines</span></span>

* <span data-ttu-id="2d9fb-110">SQLite (3,7 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="2d9fb-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="2d9fb-111">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="2d9fb-111">Limitations</span></span>

<span data-ttu-id="2d9fb-112">Zapoznaj się z [ograniczeniami oprogramowania SQLite](limitations.md) w przypadku niektórych ważnych ograniczeń dostawcy oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="2d9fb-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
