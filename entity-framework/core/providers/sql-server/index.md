---
title: Dostawca bazy danych serwera programu Microsoft SQL - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995672"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Dostawca bazy danych programu Microsoft SQL Server EF Core

Ten dostawca bazy danych umożliwia platformy Entity Framework Core do użycia z programem Microsoft SQL Server (w tym usługi SQL Azure). Dostawca jest przechowywane jako część [projektu programu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakietu Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Rozpocznij

Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.
* [Rozpoczynanie pracy w programie .NET Framework (Konsola, WinForms, WPF, itd.)](../../get-started/full-dotnet/index.md)

* [Wprowadzenie do programu ASP.NET Core](../../get-started/aspnetcore/index.md)

* [UnicornStore przykładowej aplikacji](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Obsługiwane baz danych

* Microsoft SQL Server (2008 lub nowszy)

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszy)

* .NET Core

* Narzędzie mono (4.2.0 lub nowszy)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
