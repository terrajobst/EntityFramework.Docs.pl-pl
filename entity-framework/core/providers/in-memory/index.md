---
title: Dostawca bazy danych inMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502126"
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core dostawcy bazy danych w pamięci

Ten dostawca bazy danych umożliwia Entity Framework Core używany z bazą danych w pamięci. Może to być przydatne do testowania, chociaż dostawca programu SQLite w trybie w pamięci może być bardziej odpowiednim zamiennikiem testu dla relacyjnych baz danych. Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalacja programu

Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

### <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

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
