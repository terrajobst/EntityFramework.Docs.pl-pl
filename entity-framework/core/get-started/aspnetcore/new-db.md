---
title: Wprowadzenie do platformy ASP.NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 803b0b71b2a2093432d76bc159875d65ab379b9a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489299"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Wprowadzenie do programu EF Core programu ASP.NET Core przy użyciu nowej bazy danych

W tym samouczku utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane dostępu przy użyciu platformy Entity Framework Core. W samouczku migracje utworzyć bazę danych z modelu danych.

Za pomocą programu Visual Studio 2017 na Windows lub za pomocą interfejsu wiersza polecenia platformy .NET Core w systemie Windows, macOS lub Linux, można wykonać kroki samouczka.

Wyświetl przykład w tym artykule w witrynie GitHub:
* [Program Visual Studio 2017 z programem SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [.NET core interfejsu wiersza polecenia za pomocą SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:
  * **ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)
  * **Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Otwórz program Visual Studio 2017
* **Plik > Nowy > Projekt**
* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Wprowadź **EFGetStarted.AspNetCore.NewDb** nazwę i kliknij przycisk **OK**.
* W **Nowa aplikacja internetowa ASP.NET Core** okno dialogowe:
  * Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 2.1** wybranych z listy rozwijanej
  * Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu projektu
  * Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**
  * Kliknij przycisk **OK**

Ostrzeżenie: Jeśli używasz **indywidualne konta użytkowników** zamiast **Brak** dla **uwierzytelniania** , a następnie na model Entity Framework Core zostanie dodany do projektu w `Models\IdentityModel.cs`. Przy użyciu technik, z którego dowiesz się, w tym samouczku, można dodać drugi model lub rozszerzyć ten istniejący model zawiera Twoje klas jednostek.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Uruchom następujące polecenie, aby utworzyć projekt programu MVC:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Przejdź do katalogu projektu. Kolejne polecenia, które możesz wprowadzić należy uruchamiać dla nowego projektu.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

Aby zainstalować programu EF Core, należy zainstalować pakiet dla dostawców bazy danych programu EF Core, który ma pod kątem. Aby uzyskać listę dostępnych dostawców, zobacz [dostawcy baz danych](../../providers/index.md). 

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Na potrzeby tego samouczka nie trzeba zainstalować pakiet dostawcy, ponieważ w tym samouczku użyto programu SQL Server. Pakiet dostawcy programu SQL Server znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Ten samouczek używa bazy danych SQLite, ponieważ jest uruchamiana na wszystkich platformach, które obsługuje platformy .NET Core.

* Uruchom następujące polecenie, aby zainstalować dostawcę bazy danych SQLite:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Tworzenie modelu

Zdefiniuj klasę kontekstu i klas jednostek, które tworzą model.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy **modeli** i wybierz polecenie **Dodaj > klasa**.
* Wprowadź **Model.cs** jako nazwę i kliknij przycisk **OK**.
* Zastąp zawartość pliku następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* W **modeli** Utwórz folder **Model.cs** następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Zazwyczaj jest aplikacji produkcyjnej przełączyć każda klasa w oddzielnym pliku. Dla uproszczenia ten samouczek umieszcza w ramach tych zajęć w jednym pliku.

## <a name="register-the-context-with-dependency-injection"></a>Zarejestrowanie kontekście wstrzykiwanie zależności

Usługi (takie jak `BloggingContext`) zostały zarejestrowane przy użyciu [wstrzykiwanie zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) podczas uruchamiania aplikacji. Składniki, które wymagają tych usług, (na przykład kontrolerów MVC) znajdują się tych usług za pomocą właściwości lub parametry konstruktora.

Zapewnienie `BloggingContext` dostępne dla kontrolerów MVC, zarejestruj go jako usługę.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **Startup.cs** Dodaj następujący kod `using` instrukcji:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Dodaj następujący wyróżniony kod do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* W **Startup.cs** Dodaj następujący kod `using` instrukcji:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Dodaj następujący wyróżniony kod do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Aplikacji produkcyjnej zazwyczaj umieścić ciąg połączenia w zmiennej środowisku lub pliku konfiguracji. Dla uproszczenia w tym samouczku zdefiniowano go w kodzie. Zobacz [parametry połączenia](../../miscellaneous/connection-strings.md) Aby uzyskać więcej informacji.

## <a name="create-the-database"></a>Tworzenie bazy danych

Następujące kroki użycia [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**
* Uruchom następujące polecenia:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Jeśli zostanie wyświetlony błąd wskazujący `The term 'add-migration' is not recognized as the name of a cmdlet`, zamknij i otwórz ponownie program Visual Studio.

  `Add-Migration` Polecenia scaffolds migracji, aby utworzyć początkowy zestaw tabel dla modelu. `Update-Database` Polecenie tworzy bazę danych i dotyczy nowej migracji.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Uruchom następujące polecenia:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  `migrations` Polecenia scaffolds migracji, aby utworzyć początkowy zestaw tabel dla modelu. `database update` Polecenie tworzy bazę danych i dotyczy nowej migracji.

---

## <a name="create-a-controller"></a>Tworzenie kontrolera

Tworzenia szkieletu kontrolera i widoki dla `Blog` jednostki.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.
* Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Dodaj**.
* Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**.
* Kliknij przycisk **Dodaj**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Uruchom następujące polecenia:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
  ```

  `tool install` i `add package` polecenia instalacji narzędzia, które można tworzenia szkieletu, widoków i kontrolerów. `restore` Polecenia zapewnia, że wszystkie pakiety projektu zostaną pobrane, a `aspnet-codegenerator` polecenie powoduje szkieletu.
---

Aparat tworzenia szkieletów tworzy następujące pliki:

* Kontroler (*Controllers/BlogsController.cs*)
* Widokami razor dla stron Create, Delete, szczegółowe informacje, edycji i indeksu (_Views/Movies/*.cshtml_)

## <a name="run-the-application"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Debugowanie** > **Uruchom bez debugowania**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet run
```
---

* Przejdź do `/Blogs`

* Użyj **Utwórz nowy** link, aby utworzyć wpisy na blogu.

  ![Tworzenie strony](_static/create.png)

* Test **szczegóły**, **Edytuj**, i **Usuń** łącza.

  ![Strona indeksu](_static/index-new-db.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [EF - nowej bazy danych za pomocą SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.
* [Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
