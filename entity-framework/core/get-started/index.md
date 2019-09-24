---
title: Wprowadzenie — EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 41ebdcbb3f51c914ee7befb3c1a9c0042e9b43c8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196902"
---
# <a name="getting-started-with-ef-core"></a>Wprowadzenie z EF Core

W tym samouczku utworzysz aplikację konsolową .NET Core, która zapewnia dostęp do danych w bazie danych programu SQLite przy użyciu Entity Framework Core.

Możesz wykonać czynności opisane w samouczku za pomocą programu Visual Studio w systemie Windows lub za pomocą interfejs wiersza polecenia platformy .NET Core w systemie Windows, macOS lub Linux.

[Zapoznaj się z przykładem tego artykułu w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [Zestaw SDK platformy .NET Core 3,0](https://www.microsoft.com/net/download/core).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Program Visual Studio 2019 w wersji 16,3 lub nowszej](https://www.visualstudio.com/downloads/) z tym obciążeniem:
  * **Programowanie dla wielu platform w środowisku .NET Core** (w obszarze **inne zestawy narzędzi**)

---

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

``` Console
dotnet new console -o EFGetStarted
cd EFGetStarted
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Otwórz program Visual Studio
* Kliknij pozycję **Utwórz nowy projekt**
* Wybierz pozycję **aplikacja konsoli (.NET Core)** z **C#** tagiem i kliknij przycisk **dalej** .
* Wprowadź **EFGetStarted** dla nazwy i kliknij przycisk **Utwórz** .

---

## <a name="install-entity-framework-core"></a>Zainstaluj Entity Framework Core

Aby zainstalować EF Core, należy zainstalować pakiet dla dostawców usługi EF Core Database, które mają być przeznaczone do celów. W tym samouczku jest używane oprogramowanie SQLite, ponieważ jest ono wykonywane na wszystkich platformach obsługiwanych przez platformę .NET Core. Aby uzyskać listę dostępnych dostawców, zobacz [dostawcy bazy danych](../providers/index.md).

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**
* Uruchom następujące polecenia:

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

Wyowietlon Możesz również zainstalować pakiety, klikając prawym przyciskiem myszy projekt i wybierając pozycję **Zarządzaj pakietami NuGet** .

---

## <a name="create-the-model"></a>Tworzenie modelu

Zdefiniuj klasę kontekstu i klasy jednostek, które tworzą model.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* W katalogu projektu Utwórz **model.cs** z następującym kodem

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj klasy >**
* Wprowadź **model.cs** jako nazwę, a następnie kliknij przycisk **Dodaj** .
* Zastąp zawartość pliku następującym kodem

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

EF Core [może również odtworzyć](../managing-schemas/scaffolding.md) model z istniejącej bazy danych.

Wyowietlon W rzeczywistej aplikacji należy umieścić każdą klasę w osobnym pliku i umieścić [Parametry połączenia](../miscellaneous/connection-strings.md) w pliku konfiguracyjnym lub zmiennej środowiskowej. Aby zachować ten samouczek, wszystko jest zawarte w jednym pliku.

## <a name="create-the-database"></a>Tworzenie bazy danych

Poniższe kroki służą do tworzenia bazy [danych programu.](xref:core/managing-schemas/migrations/index)

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Uruchom następujące polecenia:

  ``` Console
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Spowoduje to zainstalowanie programu [dotnet EF](../miscellaneous/cli/dotnet.md) i pakietu projektowego, który jest wymagany do uruchomienia polecenia w projekcie. `migrations` Polecenie tworzy szkielet migracji w celu utworzenia początkowego zestawu tabel dla modelu. `database update` Polecenie tworzy bazę danych i stosuje do niej nową migrację.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Uruchom następujące polecenia w **konsoli Menedżera pakietów**

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  Spowoduje to zainstalowanie [narzędzi PMC dla EF Core](../miscellaneous/cli/powershell.md). `Add-Migration` Polecenie tworzy szkielet migracji w celu utworzenia początkowego zestawu tabel dla modelu. `Update-Database` Polecenie tworzy bazę danych i stosuje do niej nową migrację.

---

## <a name="create-read-update--delete"></a>Tworzenie, odczytywanie, aktualizowanie & Usuwanie

* Otwórz *program.cs* i Zastąp zawartość następującym kodem:

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

``` Console
dotnet run
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Program Visual Studio używa niespójnego katalogu roboczego podczas uruchamiania aplikacji konsolowych platformy .NET Core. (zobacz [dotnet/Project-System # 3619](https://github.com/dotnet/project-system/issues/3619)) Powoduje to zgłaszanie wyjątku: *nie ma takiej tabeli: Blogi*. Aby zaktualizować katalog roboczy:

* Kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Edytuj plik projektu**
* Po prostu poniżej właściwości *TargetFramework* Dodaj następujące elementy:

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* Zapisz plik

Teraz możesz uruchomić aplikację:

* **Debugowanie > uruchamiane bez debugowania**

---

# <a name="next-steps"></a>Następne kroki

* Postępuj zgodnie z [samouczkiem ASP.NET Core](/aspnet/core/data/ef-rp/intro) , aby użyć EF Core w aplikacji sieci Web
* Dowiedz się więcej o [wyrażeniach zapytań LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
* [Skonfiguruj model](xref:core/modeling/index) , aby określić elementy, takie jak [wymagana](xref:core/modeling/required-optional) i [Maksymalna długość](xref:core/modeling/max-length)
* Użyj [migracji](xref:core/managing-schemas/migrations/index) , aby zaktualizować schemat bazy danych po zmianie modelu
