---
title: Wprowadzenie do platformy ASP.NET Core — Nowa baza danych — EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996067"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Wprowadzenie do programu EF Core programu ASP.NET Core przy użyciu nowej bazy danych

W tym samouczku utworzysz aplikację ASP.NET Core MVC, który wykonuje podstawowe dane dostępu przy użyciu platformy Entity Framework Core. Migracje umożliwia tworzenie bazy danych z modelu platformy EF Core.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj następujące oprogramowanie:

* [Visual Studio 2017 w wersji 15.7](https://www.visualstudio.com/downloads/) za pomocą tych obciążeniach:
  * **ASP.NET i tworzenie aplikacji internetowych** (w obszarze **sieć Web i chmura**)
  * **Programowanie dla wielu platform .NET core** (w obszarze **inne zestawy narzędzi**)
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-a-new-project-in-visual-studio-2017"></a>Utwórz nowy projekt w programie Visual Studio 2017

* Otwórz program Visual Studio 2017
* **Plik > Nowy > Projekt**
* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Wprowadź **EFGetStarted.AspNetCore.NewDb** nazwę i kliknij przycisk **OK**.
* W **Nowa aplikacja internetowa ASP.NET Core** okno dialogowe:
  * Upewnij się, opcje **platformy .NET Core** i **platformy ASP.NET Core 2.1** wybranych z list rozwijanych
  * Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu projektu
  * Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**
  * Kliknij przycisk **OK**

Ostrzeżenie: Jeśli używasz **indywidualne konta użytkowników** zamiast **Brak** dla **uwierzytelniania** , a następnie na model Entity Framework Core zostanie dodany do projektu w `Models\IdentityModel.cs`. Przy użyciu technik, z którego dowiesz się, w tym samouczku, można dodać drugi model lub rozszerzyć ten istniejący model zawiera Twoje klas jednostek.

## <a name="install-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

Aby zainstalować programu EF Core, należy zainstalować pakiet dla dostawców bazy danych programu EF Core, który ma pod kątem. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md). 

Na potrzeby tego samouczka nie trzeba zainstalować pakiet dostawcy, ponieważ w tym samouczku użyto programu SQL Server. Pakiet dostawcy programu SQL Server znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="create-the-model"></a>Tworzenie modelu

Zdefiniuj klasę kontekstu i klas jednostek, które tworzą model:

* Kliknij prawym przyciskiem myszy **modeli** i wybierz polecenie **Dodaj > klasa**.
* Wprowadź **Model.cs** jako nazwę i kliknij przycisk **OK**.
* Zastąp zawartość pliku następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

W rzeczywistych aplikacjach przeważnie przełączyć każdej klasy z modelu w oddzielnym pliku. Dla uproszczenia ten samouczek umieszcza wszystkie klasy w jednym pliku.

## <a name="register-your-context-with-dependency-injection"></a>Zarejestrowanie kontekst wstrzykiwanie zależności

Usługi (takie jak `BloggingContext`) zostały zarejestrowane przy użyciu [wstrzykiwanie zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) podczas uruchamiania aplikacji. Składniki, które wymagają tych usług, (na przykład kontrolerach MVC) znajdują się następnie tych usług za pomocą właściwości lub parametry konstruktora.

Zapewnienie `BloggingContext` dostępne dla kontrolerów MVC, zarejestruj go jako usługę.

* Otwórz **Startup.cs**
* Dodaj następujący kod `using` instrukcji:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Wywołaj `AddDbContext` metodę, aby zarejestrować kontekst jako usługa.

* Dodaj następujący wyróżniony kod do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Uwaga: Rzeczywistej aplikacji ogólnie umieścić ciąg połączenia w zmiennej środowisku lub pliku konfiguracji. Dla uproszczenia w tym samouczku zdefiniowano go w kodzie. Zobacz [parametry połączenia](../../miscellaneous/connection-strings.md) Aby uzyskać więcej informacji.

## <a name="create-the-database"></a>Tworzenie bazy danych

Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) utworzyć bazę danych.

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**
* Uruchom `Add-Migration InitialCreate` do tworzenia szkieletu migracji, aby utworzyć początkowy zestaw tabel dla modelu. Jeśli zostanie wyświetlony błąd wskazujący `The term 'add-migration' is not recognized as the name of a cmdlet`, zamknij i otwórz ponownie program Visual Studio.
* Uruchom `Update-Database` zastosować nową migrację do bazy danych. To polecenie tworzy bazę danych przed zastosowaniem migracji.

## <a name="create-a-controller"></a>Tworzenie kontrolera

Tworzenia szkieletu kontrolera i widoki dla `Blog` jednostki.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.
* Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Dodaj**.
* Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**.
* Kliknij przycisk **Dodaj**.


## <a name="run-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby uruchomić i przetestować aplikację.

* Przejdź do `/Blogs`
* Użyj linku, aby utworzyć wpisy na blogu. Szczegóły testu, a następnie usuń linki.

![obraz](_static/create.png)

![obraz](_static/index-new-db.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [EF - nowej bazy danych za pomocą SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.
* [Wprowadzenie do platformy ASP.NET Core MVC na komputerze Mac lub Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
