---
title: "Dostawca bazy danych programu Microsoft SQL Server Compact Edition — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Dostawca bazy danych programu Microsoft SQL Server Compact Edition EF Core

Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z programem SQL Server Compact Edition. Dostawca jest przechowywany jako część [ErikEJ/EntityFramework.SqlServerCompact GitHub projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core. Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.

## <a name="install"></a>Zainstaluj

Aby pracować z programu SQL Server Compact Edition 4.0, zainstaluj [pakietu EntityFrameworkCore.SqlServerCompact40 NuGet](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Aby pracować z programu SQL Server Compact Edition 3.5, zainstaluj [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Rozpocznij

Zobacz [wprowadzenie dokumentacji w witrynie projektu](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

## <a name="supported-database-engines"></a>Obsługiwanych aparatów bazy danych

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszej)
