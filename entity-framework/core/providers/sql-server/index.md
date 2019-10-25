---
title: Dostawca bazy danych Microsoft SQL Server — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812104"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca bazy danych EF Core Microsoft SQL Server

Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Microsoft SQL Server (w tym SQL Azure). Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Od wersji 3.0.0 dostawca odwołuje się do Microsoft. Data. SqlClient (poprzednie wersje zależały od system. Data. SqlClient). Jeśli projekt przejmuje bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do poprawnego pakietu.

## <a name="supported-database-engines"></a>Obsługiwane aparaty bazy danych

* Microsoft SQL Server (2012 lub nowsza)
