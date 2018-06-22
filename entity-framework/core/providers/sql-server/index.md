---
title: Dostawca bazy danych serwera Microsoft SQL — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678653"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca bazy danych programu Microsoft SQL Server EF Core

Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z programu Microsoft SQL Server (w tym usług SQL Azure). Dostawca jest przechowywany jako część [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).

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
