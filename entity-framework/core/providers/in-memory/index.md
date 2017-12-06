---
title: Dostawca bazy danych InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>Dostawca programu EF podstawowej bazy danych w pamięci

Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci. Jest to przydatne podczas testowania kodu korzystającego z programu Entity Framework Core. Dostawca jest przechowywany jako część [projektu EntityFramework GitHub](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakietu Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Rozpocznij

Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.
* [Testowanie za pomocą InMemory](../../miscellaneous/testing/in-memory.md)

* [Przykładowe UnicornStore testów aplikacji](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Obsługiwanych aparatów bazy danych

* Wbudowane bazy danych w pamięci (przeznaczony tylko do celów testowych)

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszej)

* .NET Core

* Mono (4.2.0 i jego nowszych wersjach)

* Platforma uniwersalna systemu Windows
