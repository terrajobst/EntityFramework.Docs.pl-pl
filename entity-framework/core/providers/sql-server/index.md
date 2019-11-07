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
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca bazy danych EF Core Microsoft SQL Server

Ten dostawca bazy danych umożliwia używanie Entity Framework Core z Microsoft SQL Server (w tym SQL Azure). Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Od wersji 3.0.0 dostawca odwołuje się do Microsoft. Data. SqlClient (poprzednie wersje zależały od system. Data. SqlClient). Jeśli projekt przejmuje bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do poprawnego pakietu.

## <a name="supported-database-engines"></a>Obsługiwane aparaty bazy danych

* Microsoft SQL Server (2012 lub nowsza)
