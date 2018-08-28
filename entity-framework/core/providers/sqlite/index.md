---
title: Dostawca bazy danych SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994005"
---
# <a name="sqlite-ef-core-database-provider"></a>Dostawca bazy danych systemu SQLite EF Core

Ten dostawca bazy danych umożliwia platformy Entity Framework Core do użycia z bazy danych SQLite. Dostawca jest przechowywane jako część [projektu platformy Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakietu Microsoft.EntityFrameworkCore.Sqlite NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Rozpocznij

Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.
* [Lokalne bazy danych SQLite na platformy uniwersalnej systemu Windows](../../get-started/uwp/getting-started.md)

* [Aplikacji .NET core do nowej bazy danych SQLite](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker przykładowej aplikacji](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn Packer przykładowej aplikacji](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Obsługiwane baz danych

* Bazy danych SQLite (3.7 lub nowszy)

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszy)

* .NET Core

* Narzędzie mono (4.2.0 lub nowszy)

* Platforma uniwersalna systemu Windows

## <a name="limitations"></a>Ograniczenia

Zobacz [ograniczenia SQLite](limitations.md) dla pewne ważne ograniczenia dotyczące dostawcy bazy danych SQLite.
