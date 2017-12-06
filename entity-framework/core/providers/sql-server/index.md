---
title: "Dostawca bazy danych serwera Microsoft SQL — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca bazy danych programu Microsoft SQL Server EF Core

Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z programu Microsoft SQL Server (w tym usług SQL Azure). Dostawca jest przechowywany jako część [projektu EntityFramework GitHub](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakietu Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Rozpocznij

Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.
* [Rozpoczynanie pracy w programie .NET Framework (konsoli WinForms, WPF, itp.)](../../get-started/full-dotnet/index.md)

* [Wprowadzenie do platformy ASP.NET Core](../../get-started/aspnetcore/index.md)

* [UnicornStore przykładowej aplikacji](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Obsługiwanych aparatów bazy danych

* Microsoft SQL Server (2008 i nowszych wersjach)

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszej)

* .NET Core

* Mono (4.2.0 i jego nowszych wersjach)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
