---
title: Dostawca bazy danych SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417778"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="3ddc9-102">Dostawca podstawowej bazy danych SQLite EF</span><span class="sxs-lookup"><span data-stu-id="3ddc9-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="3ddc9-103">Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ddc9-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="3ddc9-104">Dostawca jest utrzymywany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="3ddc9-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="3ddc9-105">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="3ddc9-105">Install</span></span>

<span data-ttu-id="3ddc9-106">Zainstaluj [pakiet Microsoft.EntityFrameWorkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="3ddc9-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="3ddc9-107">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="3ddc9-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="3ddc9-108">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3ddc9-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="3ddc9-109">Obsługiwane aparaty baz danych</span><span class="sxs-lookup"><span data-stu-id="3ddc9-109">Supported Database Engines</span></span>

* <span data-ttu-id="3ddc9-110">SQLite (od 3,7 do góry)</span><span class="sxs-lookup"><span data-stu-id="3ddc9-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="3ddc9-111">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="3ddc9-111">Limitations</span></span>

<span data-ttu-id="3ddc9-112">Zobacz [ograniczenia SQLite](limitations.md) dla niektórych ważnych ograniczeń dostawcy SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ddc9-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
