---
title: Dostawca bazy danych programu Microsoft SQL Server — EF Core
description: Dokumentacja dostawcy bazy danych umożliwiająca program Entity Framework Core do użycia z programem Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417795"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="327df-103">Dostawca podstawowej bazy danych ef programu Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="327df-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="327df-104">Ten dostawca bazy danych umożliwia entity framework core do użycia z microsoft SQL Server (w tym usługi Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="327df-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="327df-105">Dostawca jest utrzymywany w ramach [projektu core entity framework](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="327df-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="327df-106">Instalowanie</span><span class="sxs-lookup"><span data-stu-id="327df-106">Install</span></span>

<span data-ttu-id="327df-107">Zainstaluj [pakiet Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="327df-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="327df-108">Interfejs wiersza polecenia platformy .NET Core</span><span class="sxs-lookup"><span data-stu-id="327df-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="327df-109">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="327df-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="327df-110">Od wersji 3.0.0 dostawca odwołuje się do pliku Microsoft.Data.SqlClient (poprzednie wersje zależały od pliku System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="327df-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="327df-111">Jeśli projekt ma bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do pakietu Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="327df-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="327df-112">Obsługiwane aparaty baz danych</span><span class="sxs-lookup"><span data-stu-id="327df-112">Supported Database Engines</span></span>

* <span data-ttu-id="327df-113">Program Microsoft SQL Server (od 2012 r.)</span><span class="sxs-lookup"><span data-stu-id="327df-113">Microsoft SQL Server (2012 onwards)</span></span>
