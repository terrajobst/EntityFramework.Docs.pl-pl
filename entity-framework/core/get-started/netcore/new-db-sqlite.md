---
title: Wprowadzenie do programu .NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Wprowadzenie do platformy .NET Core przy użyciu platformy Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911505"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Wprowadzenie do nowej bazy danych z programem EF Core w aplikacji Konsolowej .NET Core

W tym instruktażu utworzysz aplikację konsoli .NET Core, która wykonuje dostęp do danych w bazie danych SQLite przy użyciu platformy Entity Framework Core. Migracje umożliwia tworzenie bazy danych z modelu. Zobacz [ASP.NET Core — Nowa baza danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

[.NET Core SDK](https://www.microsoft.com/net/core) 2.1

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Utwórz nowy projekt konsoli:

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. W tym instruktażu wykorzystano bazy danych SQLite. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Uruchom `dotnet restore` zainstalować nowe pakiety.

## <a name="create-the-model"></a>Tworzenie modelu

Definiowanie klas jednostek i kontekstu, które tworzą model.

* Utwórz nową *Model.cs* pliku o następującej zawartości.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Porada: W rzeczywistej aplikacji, możesz umieścić każda klasa w oddzielnym pliku, a parametry połączenia w pliku konfiguracji. W celu uproszczenia tego samouczka wszystko, co znajduje się w jednym pliku.

## <a name="create-the-database"></a>Tworzenie bazy danych

Po utworzeniu modelu, użyj [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.

* Uruchom `dotnet ef migrations add InitialCreate` tworzenia szkieletu migracji i utworzyć początkowy zestaw tabel dla modelu.
* Uruchom `dotnet ef database update` zastosować nową migrację do bazy danych. To polecenie tworzy bazę danych przed zastosowaniem migracji.

*Blogging.db** bazy danych SQLite znajduje się w katalogu projektu.

## <a name="use-your-model"></a>Użyj modelu

* Otwórz *Program.cs* i zastąp jego zawartość następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testowanie aplikacji:

  `dotnet run`

  Blog jeden jest zapisywany w bazie danych i szczegółowe informacje o wszystkich blogów są wyświetlane w konsoli.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Zmiana modelu:

- W przypadku wprowadzenia zmian do modelu, można użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowego [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) się odpowiedni schemat zmienia się z bazą danych. Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `dotnet ef database update` polecenie, aby zastosować zmiany do bazy danych.
- Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.
- Bazy danych SQLite nie obsługuje wszystkich migracji (zmiany schematu) ze względu na ograniczenia w bazy danych SQLite. Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md). W nowych wdrożeniach należy wziąć pod uwagę porzucenie bazy danych i tworzenia nowej, a nie przy użyciu migracji po zmianie modelu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [.NET core — Nowa baza danych za pomocą SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.
* [Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
