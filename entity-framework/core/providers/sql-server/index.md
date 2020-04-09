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
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca podstawowej bazy danych ef programu Microsoft SQL Server

Ten dostawca bazy danych umożliwia entity framework core do użycia z microsoft SQL Server (w tym usługi Azure SQL Database). Dostawca jest utrzymywany w ramach [projektu core entity framework](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalowanie

Zainstaluj [pakiet Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Od wersji 3.0.0 dostawca odwołuje się do pliku Microsoft.Data.SqlClient (poprzednie wersje zależały od pliku System.Data.SqlClient). Jeśli projekt ma bezpośrednią zależność od SqlClient, upewnij się, że odwołuje się do pakietu Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Obsługiwane aparaty baz danych

* Program Microsoft SQL Server (od 2012 r.)
