---
title: Dostawca bazy danych programu SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 433dedbe1e0e11bf2fd83e259e321ece32b2c188
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824804"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="2abbe-102">Dostawca bazy danych EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="2abbe-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="2abbe-103">Ten dostawca bazy danych umożliwia Entity Framework Core używany z programem SQLite.</span><span class="sxs-lookup"><span data-stu-id="2abbe-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="2abbe-104">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2abbe-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2abbe-105">Instalacja programu</span><span class="sxs-lookup"><span data-stu-id="2abbe-105">Install</span></span>

<span data-ttu-id="2abbe-106">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="2abbe-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="2abbe-107">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2abbe-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="2abbe-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2abbe-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="2abbe-109">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="2abbe-109">Supported Database Engines</span></span>

* <span data-ttu-id="2abbe-110">SQLite (3,7 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="2abbe-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="2abbe-111">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="2abbe-111">Limitations</span></span>

<span data-ttu-id="2abbe-112">Zapoznaj się z [ograniczeniami oprogramowania SQLite](limitations.md) w przypadku niektórych ważnych ograniczeń dostawcy oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="2abbe-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
