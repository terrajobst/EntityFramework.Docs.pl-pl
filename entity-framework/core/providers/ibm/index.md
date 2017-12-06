---
title: "Dostawca danych IBM do bazy danych serwera (bazy danych DB2) — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>IBM danych serwera (bazy danych DB2) EF podstawowej bazy danych dostawców

Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z IBM danych serwera. Dostawca jest obsługiwana przez IBM.

> [!NOTE]  
> Ten dostawca nie jest obsługiwana w ramach projektu Entity Framework Core. Rozważając innych dostawców, należy ocenić jakości, licencjonowanie, obsługi, itp., aby upewnić się, że spełniają one wymagania.

## <a name="install"></a>Zainstaluj

Aby pracować z IBM danych serwera w systemie Windows, należy zainstalować [IBM. Pakiet EntityFrameworkCore NuGet](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Do pracy z IBM danych serwera w systemie Linux należy zainstalować [IBM. Pakiet NuGet EntityFrameworkCore lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Rozpocznij

[Wprowadzenie do korzystania z dostawcy .NET IBM dla platformy .NET Core](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Obsługiwanych aparatów bazy danych

* zOS
* W tym IBM dashDB LUW
* IBM I
* Programu Informix

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.6 i jego nowszych wersjach)
* .NET Core
