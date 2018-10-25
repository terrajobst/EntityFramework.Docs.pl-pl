---
title: Wprowadzenie do programu .NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
description: Wprowadzenie do platformy .NET Core przy użyciu platformy Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 6cebe14e179cb6998592f5d3823c114b3bda0138
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022314"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Wprowadzenie do nowej bazy danych z programem EF Core w aplikacji Konsolowej .NET Core

W tym samouczku utworzysz aplikację konsoli .NET Core, która wykonuje dostęp do danych w bazie danych SQLite przy użyciu platformy Entity Framework Core. Migracje umożliwia tworzenie bazy danych z modelu. Zobacz [ASP.NET Core — Nowa baza danych](xref:core/get-started/aspnetcore/new-db) dla wersji programu Visual Studio przy użyciu platformy ASP.NET Core MVC.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Wymagania wstępne

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Utwórz nowy projekt konsoli:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Zmień bieżący katalog

W kolejnych krokach samouczka, należy wydać `dotnet` poleceń dla aplikacji.

* Możemy zmienić bieżący katalog do katalogu aplikacji w następujący sposób:

  ``` Console
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

Po utworzeniu modelu, użyj [migracje](xref:core/managing-schemas/migrations/index) utworzyć bazę danych.

* Uruchom `dotnet ef migrations add InitialCreate` tworzenia szkieletu migracji i utworzyć początkowy zestaw tabel dla modelu.
* Uruchom `dotnet ef database update` zastosować nową migrację do bazy danych. To polecenie tworzy bazę danych przed zastosowaniem migracji.

*Blogging.db** bazy danych SQLite znajduje się w katalogu projektu.

## <a name="use-the-model"></a>Użyj modelu

* Otwórz *Program.cs* i zastąp jego zawartość następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testowanie aplikacji z konsoli. Zobacz [Uwaga programu Visual Studio](#vs) do uruchomienia aplikacji w programie Visual Studio.

  `dotnet run`

  Blog jeden jest zapisywany w bazie danych i szczegółowe informacje o wszystkich blogów są wyświetlane w konsoli.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Zmiana modelu:

- Jeśli wprowadzisz zmiany w modelu, można użyć `dotnet ef migrations add` polecenie, aby utworzyć szkielet nowego [migracji](xref:core/managing-schemas/migrations/index). Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `dotnet ef database update` polecenie, aby zastosować schemat zmienia się z bazą danych.
- Używa programu EF Core `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.
- Aparat bazy danych SQLite nie obsługuje niektórych zmiany schematu, które są obsługiwane w większości innych relacyjnych baz danych. Na przykład `DropColumn` operacja nie jest obsługiwana. EF Core migracji wygeneruje kod dla tych operacji. Ale Jeśli spróbujesz zastosować je do bazy danych lub wygenerować skrypt programu EF Core zgłasza wyjątek wyjątków. Zobacz [ograniczenia SQLite](../../providers/sqlite/limitations.md). W nowych wdrożeniach należy wziąć pod uwagę porzucenie bazy danych i tworzenia nowej, a nie przy użyciu migracji po zmianie modelu.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Uruchamianie z programu Visual Studio

Aby uruchomić ten przykład z programu Visual Studio, należy ustawić katalogu roboczego ręcznie, aby być w katalogu głównym projektu. Jeśli nie ustawisz katalogu roboczego następujące `Microsoft.Data.Sqlite.SqliteException` zgłaszany: `SQLite Error 1: 'no such table: Blogs'`.

Aby ustawić katalog roboczy:

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, a następnie wybierz **właściwości**.
* Wybierz **debugowania** karty w okienku po lewej stronie.
* Ustaw **katalog roboczy** do katalogu projektu.
* Zapisz zmiany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Samouczek: Rozpoczynanie pracy z programem EF Core programu ASP.NET Core przy użyciu nowej bazy danych przy użyciu systemu SQLite](xref:core/get-started/aspnetcore/new-db)
* [Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Samouczek: Strony Razor za pomocą platformy Entity Framework Core w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
