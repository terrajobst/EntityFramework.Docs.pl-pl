---
title: Wprowadzenie .NET Core - nowej bazy danych — EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Rozpoczynanie pracy z platformą .NET Core przy użyciu programu Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 2511dfa3f3262bb12c2058dc1c402b7dcc4c670d
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Wprowadzenie do podstawowych EF w aplikacji konsoli .NET Core nowej bazy danych

W tym przewodniku spowoduje utworzenie aplikacji konsoli .NET Core, który wykonuje dostęp do podstawowych danych względem bazy danych SQLite przy użyciu programu Entity Framework Core. Migracje użyje do utworzenia bazy danych z modelu. Zobacz [platformy ASP.NET Core - nową bazę danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) w witrynie GitHub.

## <a name="prerequisites"></a>Wymagania wstępne

Do przeprowadzenia tego instruktażu potrzebne są następujące wymagania wstępne:
* System operacyjny obsługuje .NET Core.
* [.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (mimo że instrukcje może służyć do tworzenia aplikacji z poprzedniej wersji z bardzo kilka zmian).

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Utwórz nową `ConsoleApp.SQLite` folderu projektu i użyj `dotnet` polecenie, aby umieścić w nim aplikacji .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Zainstaluj program Entity Framework Core

Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa. W tym przewodniku zastosowano SQLite. Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).

* Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Ręcznie Edytuj `ConsoleApp.SQLite.csproj` dodać DotNetCliToolReference do Microsoft.EntityFrameworkCore.Tools.DotNet:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` teraz powinny zawierać następujące:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Uwaga: Numery wersji użyta powyżej były prawidłowe w czasie publikowania.

*  Uruchom `dotnet restore` do zainstalowania nowych pakietów.

## <a name="create-the-model"></a>Tworzenie modelu

Definiowanie kontekstu i jednostki klasy, które tworzą modelu.

* Utwórz nową *Model.cs* pliku z następującą zawartość.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Porada: W rzeczywistej aplikacji można będzie umieścić każda klasa w osobnym pliku, a ciąg połączenia w pliku konfiguracji. Aby zachować samouczka proste, wprowadzamy wszystko w jednym pliku.

## <a name="create-the-database"></a>Utwórz bazę danych

Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) tworzenia bazy danych.

* Uruchom `dotnet ef migrations add InitialCreate` utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.
* Uruchom `dotnet ef database update` na zastosowanie nowych migracji w bazie danych. To polecenie tworzy bazy danych przed zastosowaniem migracji.

> [!NOTE]  
> Używając ścieżek względnych z SQLite ścieżka będzie względem zestawu głównego aplikacji. W tym przykładzie jest głównym pliku binarnego `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, więc będzie bazy danych SQLite w `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Użyj modelu

* Otwórz *Program.cs* i Zastąp zawartość następującym kodem:

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testowanie aplikacji:

 `dotnet run`

 Jeden blogu jest zapisywana w bazie danych i szczegółowe informacje o wszystkich blogi są wyświetlane w konsoli.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Zmiana modelu:

- Jeśli wprowadzisz zmiany w modelu, możesz użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowy [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) dokonanie odpowiedniej schematu zmiany w bazie danych. Po zaznaczeniu tej opcji szkieletu kodu (i wszelkie wymagane zmiany wprowadzone), można użyć `dotnet ef database update` polecenie, aby zastosować zmiany do bazy danych.
- Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.
- SQLite nie obsługuje wszystkich migracji (zmiany schematu) ze względu na ograniczenia w SQLite. Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md). W przypadku nowych aplikacji rozważ porzucenie bazy danych i tworzenia nowego zamiast migracje po zmianie modelu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie .NET core - nowej bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek EF konsoli i platform.
* [Wprowadzenie do platformy ASP.NET Core MVC Mac lub Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
