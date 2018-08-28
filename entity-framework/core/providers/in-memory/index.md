---
title: Dostawca InMemory bazy danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993323"
---
# <a name="ef-core-in-memory-database-provider"></a>Dostawca programu EF Core bazy danych w pamięci

Ten dostawca bazy danych umożliwia platformy Entity Framework Core ma być używany z bazą danych w pamięci. Może to być przydatne w przypadku testowania, mimo że dostawcy bazy danych SQLite w trybie w pamięci może być bardziej odpowiednie zastąpienia testu dla relacyjnych baz danych. Dostawca jest przechowywane jako część [projektu programu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakietu Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Rozpocznij

Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.
* [Testowanie za pomocą InMemory](../../miscellaneous/testing/in-memory.md)

* [Przykładowe UnicornStore testów aplikacji](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Obsługiwane baz danych

* Wbudowane bazy danych w pamięci (przeznaczony tylko do celów testowych)

## <a name="supported-platforms"></a>Obsługiwane platformy

* .NET framework (4.5.1 lub nowszy)

* .NET Core

* Narzędzie mono (4.2.0 lub nowszy)

* Platforma uniwersalna systemu Windows
