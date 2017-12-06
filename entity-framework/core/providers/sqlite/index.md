---
title: "Dostawca bazy danych SQLite — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a>Dostawca bazy danych podstawowych EF SQLite

Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych SQLite. Dostawca jest przechowywany jako część [projektu EntityFramework GitHub](https://github.com/aspnet/EntityFramework).

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

* [Unicorn pakujący przykładowej aplikacji](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Obsługiwanych aparatów bazy danych

* SQLite (3.7 lub nowszej)

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszej)

* .NET Core

* Mono (4.2.0 i jego nowszych wersjach)

* Platforma uniwersalna systemu Windows

## <a name="limitations"></a>Ograniczenia

Zobacz [ograniczenia SQLite](limitations.md) dla pewne ważne ograniczenia dostawcy bazy danych SQLite.
