---
title: Dostawca bazy danych InMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417220"
---
# <a name="ef-core-in-memory-database-provider"></a>Dostawca baz danych EF Core w pamięci

Ten dostawca bazy danych umożliwia Entity Framework Core do użycia z bazy danych w pamięci. Może to być przydatne do testowania, chociaż dostawca SQLite w trybie w pamięci może być bardziej odpowiednie zastąpienie testu dla relacyjnych baz danych. Dostawca jest utrzymywany w ramach [projektu core entity framework](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalowanie

Zainstaluj [pakiet Microsoft.EntityFrameWorkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

### <a name="net-core-cli"></a>[Interfejs wiersza polecenia platformy .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[Program Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Rozpoczęcie pracy

Poniższe zasoby pomogą Ci rozpocząć pracę z tym dostawcą.

* [Testowanie za pomocą InMemory](../../miscellaneous/testing/in-memory.md)
* [Testy przykładowych aplikacji UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Obsługiwane aparaty baz danych

Baza danych pamięci w trakcie procesu (przeznaczona wyłącznie do celów testowych)
