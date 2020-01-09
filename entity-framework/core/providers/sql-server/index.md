---
title: Dostawca bazy danych Microsoft SQL Server — EF Core
description: Dokumentacja dostawcy bazy danych, która umożliwia Entity Framework Core, które mają być używane z Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502269"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca bazy danych EF Core Microsoft SQL Server

Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Microsoft SQL Server (w tym Azure SQL Database). Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalacja programu

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Od wersji 3.0.0 dostawca odwołuje się do Microsoft. Data. SqlClient (poprzednie wersje zależały od system. Data. SqlClient). Jeśli projekt pobiera bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do pakietu Microsoft. Data. SqlClient.

## <a name="supported-database-engines"></a>Obsługiwane aparaty bazy danych

* Microsoft SQL Server (2012 lub nowsza)
