---
title: "Dostawca bazy danych MySQL — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a>Dostawca bazy danych programu MySQL EF Core

Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z programem MySQL. Dostawca jest przechowywany jako część [projektu MySQL](http://dev.mysql.com).

> [!WARNING]  
> Ten dostawca jest wersji wstępnej.

> [!NOTE]  
> Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core. Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.

## <a name="install"></a>Zainstaluj

Zainstaluj [pakietu MySql.Data.EntityFrameworkCore NuGet](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Rozpocznij

Zobacz [począwszy od dostawcy MySQL EF Core i Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

## <a name="supported-database-engines"></a>Obsługiwanych aparatów bazy danych

* MySQL

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszej)

* .NET Core

Pamiętaj zapoznać się z dokumentacją MySQL, aby uzyskać informacje dotyczące zgodności wersji [tutaj](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) i [tutaj](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)
