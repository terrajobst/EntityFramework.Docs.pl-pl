---
title: Wprowadzenie do programu .NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
description: Wprowadzenie do platformy .NET Core przy użyciu platformy Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993695"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Wprowadzenie do nowej bazy danych z programem EF Core w aplikacji Konsolowej .NET Core

W tym samouczku utworzysz aplikację konsoli .NET Core, która wykonuje dostęp do danych w bazie danych SQLite przy użyciu platformy Entity Framework Core. Migracje umożliwia tworzenie bazy danych z modelu. Zobacz [ASP.NET Core — Nowa baza danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.

Wyświetlanie przykładowych w tym artykule w witrynie GitHub] (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Wymagania wstępne

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Utwórz nowy projekt konsoli:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. W tym instruktażu wykorzystano bazy danych SQLite. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* Zainstaluj Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameworkCore.Design

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Uruchom `dotnet restore` zainstalować nowe pakiety.

## <a name="create-the-model"></a>Tworzenie modelu

Definiowanie klas jednostek i kontekstu, które tworzą model.

* Utwórz nową *Model.cs* pliku o następującej zawartości.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Porada: W rzeczywistej aplikacji, możesz umieścić każda klasa w oddzielnym pliku, a parametry połączenia w zmiennej środowisku lub pliku konfiguracji. W celu uproszczenia tego samouczka wszystko, co znajduje się w jednym pliku.

## <a name="create-the-database"></a>Tworzenie bazy danych

Po utworzeniu modelu, użyj [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.

* Uruchom `dotnet ef migrations add InitialCreate` tworzenia szkieletu migracji i utworzyć początkowy zestaw tabel dla modelu.
* Uruchom `dotnet ef database update` zastosować nową migrację do bazy danych. To polecenie tworzy bazę danych przed zastosowaniem migracji.

*Blogging.db** bazy danych SQLite znajduje się w katalogu projektu.

## <a name="use-the-model"></a>Użyj modelu

* Otwórz *Program.cs* i zastąp jego zawartość następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testowanie aplikacji:

  `dotnet run`

  Blog jeden jest zapisywany w bazie danych i szczegółowe informacje o wszystkich blogów są wyświetlane w konsoli.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Zmiana modelu:

- Jeśli wprowadzisz zmiany w modelu, można użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowego [migracji](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations). Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `dotnet ef database update` polecenie, aby zastosować schemat zmienia się z bazą danych.
- Używa programu EF Core `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.
- Aparat bazy danych SQLite nie obsługuje niektórych zmiany schematu, które są obsługiwane w większości innych relacyjnych baz danych. Na przykład `DropColumn` operacja nie jest obsługiwana. EF Core migracji wygeneruje kod dla tych operacji. Ale Jeśli spróbujesz zastosować je do bazy danych lub wygenerować skrypt programu EF Core zgłasza wyjątek wyjątków. Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md). W nowych wdrożeniach należy wziąć pod uwagę porzucenie bazy danych i tworzenia nowej, a nie przy użyciu migracji po zmianie modelu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
