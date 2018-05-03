---
title: Wprowadzenie do platformy ASP.NET Core - nowej bazy danych — EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Wprowadzenie do podstawowych EF na platformy ASP.NET Core nowej bazy danych

W tym przykładzie utworzysz aplikacji platformy ASP.NET Core MVC, który wykonuje dostęp do podstawowych danych przy użyciu programu Entity Framework Core. Migracje użyje do utworzenia bazy danych z modelu EF Core. Zobacz [dodatkowe zasoby](#additional-resources) dla więcej samouczków Entity Framework Core.

Ten samouczek wymaga:
* [15 ustęp 3 programu Visual Studio 2017](https://www.visualstudio.com/downloads/) z te obciążenia:
  * **ASP.NET i sieć web development** (w obszarze **sieci Web & chmurze**)
  * **Programowanie wieloplatformowych .NET core** (w obszarze **inne procesami**)
* [.NET core 2.0 SDK](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) w witrynie GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Utwórz nowy projekt w programie Visual Studio 2017 r.

* **Plik > Nowy > Projekt**
* Wybierz z menu po lewej stronie **zainstalowana > Szablony > Visual C# > .NET Core**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Wprowadź **EFGetStarted.AspNetCore.NewDb** dla nazwy i kliknij przycisk **OK**.
* W **nową aplikację sieci Web Core ASP.NET** okna dialogowego:
  * Upewnij się, opcje **.NET Core** i **ASP.NET Core 2.0** są wybrane na liście rozwijanej listy
  * Wybierz **aplikacji sieci Web (Model-View-Controller)** szablonu projektu
  * Upewnij się, że **uwierzytelniania** ustawiono **bez uwierzytelniania**
  * Kliknij przycisk **OK**

Ostrzeżenie: Jeśli używasz **indywidualnych kont użytkowników** zamiast **Brak** dla **uwierzytelniania** , a następnie modelu Entity Framework Core zostanie dodany do projektu w `Models\IdentityModel.cs`. Korzystanie z metod, które przedstawiono w ramach tego przewodnika, można dodać drugiego modelu lub rozszerzyć istniejącego modelu zawiera klas jednostek.

## <a name="install-entity-framework-core"></a>Zainstaluj program Entity Framework Core

Zainstaluj pakiet dla EF Core powszechne bazy danych, który ma być docelowa. W tym przewodniku zastosowano programu SQL Server. Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).

* **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Użyjemy niektóre narzędzia Entity Framework Core utworzyć bazę danych z modelu EF Core. Dlatego zostanie zainstalowany pakiet narzędzi również:

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

Użyjemy niektóre platformy ASP.NET Core szkieletów narzędzia do tworzenia widoków i kontrolerów później. Dlatego zostanie zainstalowany ten pakiet projektu:

* Uruchom `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="create-the-model"></a>Tworzenie modelu

Definiowanie kontekstu i jednostki klasy, które tworzą modelu:

* Kliknij prawym przyciskiem myszy **modele** i wybierz polecenie **Dodaj > klasy**.
* Wprowadź **Model.cs** jako nazwy i kliknij przycisk **OK**.
* Zastąp zawartość pliku następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Uwaga: W rzeczywistej aplikacji zwykle przełączyć każdej klasy z modelu w oddzielnym pliku. Dla uproszczenia wprowadzamy wszystkie klasy w jednym pliku w tym samouczku.

## <a name="register-your-context-with-dependency-injection"></a>Zarejestruj kontekst iniekcji zależności

Usługi (takie jak `BloggingContext`) są zarejestrowane w usłudze [iniekcji zależności](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) podczas uruchamiania aplikacji. Składniki, które wymagają tych usług (takich jak kontrolerów MVC) są następnie udostępniane tych usług za pomocą konstruktora parametry lub właściwości.

Aby naszych kontrolerów MVC korzystać z `BloggingContext` firma Microsoft będzie rejestrowany jako usługa.

* Otwórz **Startup.cs**
* Dodaj następujące `using` instrukcji:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Dodaj `AddDbContext` metody, aby zarejestrować go w trybie usługi:

* Dodaj następujący kod do `ConfigureServices` metody:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Uwaga: Rzeczywiste aplikacji zwykle spowodowałaby parametry połączenia w pliku konfiguracji. Dla uproszczenia możemy są definiowane w kodzie. Zobacz [parametry połączenia](../../miscellaneous/connection-strings.md) Aby uzyskać więcej informacji.

## <a name="create-your-database"></a>Tworzenie bazy danych

Po utworzeniu modelu, można użyć [migracje](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) tworzenia bazy danych.

* Otwórz PMC:

  **Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów**
* Uruchom `Add-Migration InitialCreate` Aby utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu. Jeśli zostanie wyświetlony błąd informujący o `The term 'add-migration' is not recognized as the name of a cmdlet`, zamknij i otwórz ponownie program Visual Studio.
* Uruchom `Update-Database` na zastosowanie nowych migracji w bazie danych. To polecenie tworzy bazy danych przed zastosowaniem migracji.

## <a name="create-a-controller"></a>Tworzenie kontrolera

Aktywują szkielet do projektu:

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.
* Wybierz **minimalnego zależności** i kliknij przycisk **Dodaj**.
* Można zignorować lub usunąć *ScaffoldingReadMe.txt* pliku.

Teraz, gdy jest włączona funkcja szkieletów, możemy utworzyć szkielet kontrolera dla `Blog` jednostki.

* Kliknij prawym przyciskiem myszy **kontrolerów** folderu w **Eksploratora rozwiązań** i wybierz **Dodaj > kontrolera**.
* Wybierz **kontroler MVC z widokami używający narzędzia Entity Framework** i kliknij przycisk **Ok**.
* Ustaw **klasa modelu** do **Blog** i **klasa kontekstu danych** do **BloggingContext**.
* Kliknij przycisk **Dodaj**.


## <a name="run-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby uruchomić i przetestować aplikację.

* Przejdź do `/Blogs`
* Utwórz łącze umożliwia utworzenie niektórych wpisów. Testowanie szczegóły i usunąć łącza.

![obraz](_static/create.png)

![obraz](_static/index-new-db.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [EF - nowej bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek EF konsoli i platform.
* [Wprowadzenie do platformy ASP.NET Core MVC Mac lub Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Wprowadzenie do platformy ASP.NET Core MVC za pomocą programu Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Rozpoczynanie pracy z platformą ASP.NET Core i programem Entity Framework Core przy użyciu programu Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
