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
# <a name="sqlite-ef-core-database-provider"></a>Dostawca podstawowej bazy danych SQLite EF

Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z SQLite. Dostawca jest utrzymywany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalowanie

Zainstaluj [pakiet Microsoft.EntityFrameWorkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Obsługiwane aparaty baz danych

* SQLite (od 3,7 do góry)

## <a name="limitations"></a>Ograniczenia

Zobacz [ograniczenia SQLite](limitations.md) dla niektórych ważnych ograniczeń dostawcy SQLite.
