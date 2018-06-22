---
title: Dostawca bazy danych InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678989"
---
# <a name="ef-core-in-memory-database-provider"></a>Dostawca programu EF podstawowej bazy danych w pamięci

Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci. Może to być przydatne podczas testowania, chociaż dostawcy bazy danych SQLite w trybie w pamięci może być bardziej odpowiednie zastąpienia testu relacyjnych baz danych. Dostawca jest przechowywany jako część [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).

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
