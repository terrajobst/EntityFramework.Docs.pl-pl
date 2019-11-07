---
title: Dostawca bazy danych inMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 4b35e8c4b29a951449d4a26c6e274eb3015069bc
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656026"
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core dostawcy bazy danych w pamięci

Ten dostawca bazy danych umożliwia Entity Framework Core używany z bazą danych w pamięci. Może to być przydatne do testowania, chociaż dostawca programu SQLite w trybie w pamięci może być bardziej odpowiednim zamiennikiem testu dla relacyjnych baz danych. Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Zainstaluj

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Rozpocznij

Poniższe zasoby pomogą Ci rozpocząć pracę z tym dostawcą.

* [Testowanie za pomocą InMemory](../../miscellaneous/testing/in-memory.md)
* [Testy przykładowej aplikacji UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Obsługiwane aparaty bazy danych

Baza danych pamięci w procesie (przeznaczona tylko do celów testowych)
