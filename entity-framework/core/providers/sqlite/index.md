---
title: Dostawca bazy danych programu SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502256"
---
# <a name="sqlite-ef-core-database-provider"></a>Dostawca bazy danych EF Core SQLite

Ten dostawca bazy danych umożliwia Entity Framework Core używany z programem SQLite. Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalacja programu

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Obsługiwane aparaty bazy danych

* SQLite (3,7 lub nowszy)

## <a name="limitations"></a>Ograniczenia

Zapoznaj się z [ograniczeniami oprogramowania SQLite](limitations.md) w przypadku niektórych ważnych ograniczeń dostawcy oprogramowania SQLite.
