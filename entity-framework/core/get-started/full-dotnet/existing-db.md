---
title: Wprowadzenie do platformy .NET Framework — istniejącej bazy danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614418"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Wprowadzenie do programu EF Core w programie .NET Framework przy użyciu istniejącej bazy danych

W tym samouczku utworzysz aplikację konsolową, która wykonuje dostęp do podstawowych danych względem bazy danych programu Microsoft SQL Server przy użyciu platformy Entity Framework. Aby utworzyć model Entity Framework odtwarzanie istniejącej bazy danych.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Utwórz bazę danych do obsługi blogów

W tym samouczku **do obsługi blogów** bazy danych w wystąpieniu LocalDb jako istniejącej bazy danych. Jeśli masz już utworzoną **do obsługi blogów** bazy danych jako część innego samouczek, należy pominąć tę procedurę.

* Otwórz program Visual Studio

* **Narzędzia > nawiązać połączenie z bazą danych...**

* Wybierz **programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**

* Wprowadź **(localdb) \mssqllocaldb** jako **nazwy serwera**

* Wprowadź **wzorca** jako **Nazwa bazy danych** i kliknij przycisk **OK**

* Baza danych master jest teraz wyświetlany w obszarze **połączeń danych** w **Eksploratora serwera**

* Kliknij prawym przyciskiem myszy w bazie danych w **Eksploratora serwera** i wybierz **nowe zapytanie**

* Skopiuj skrypt w edytorze zapytań

* Kliknij prawym przyciskiem myszy w edytorze zapytań, a następnie wybierz pozycję **wykonania**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio 2017

* **Plik > Nowy > Projekt...**

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Desktop**

* Wybierz **Aplikacja konsoli (.NET Framework)** szablonu projektu

* Upewnij się, że projekt jest ukierunkowany **platformy .NET Framework 4.6.1** lub nowszej

* Nadaj projektowi nazwę *ConsoleApp.ExistingDb* i kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. Ten samouczek używa programu SQL Server. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

W następnym kroku użyjesz niektórych narzędzi Entity Framework Tools do odtworzenia bazy danych. Więc Zainstaluj również pakiet narzędzi.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-the-model"></a>Odtwarzanie modelu

Teraz nadszedł czas na tworzenie modelu platformy EF, w oparciu o istniejącą bazę danych.

* **Narzędzia -> pakietu NuGet Manager -> Konsola Menedżera pakietów**

* Uruchom następujące polecenie, aby utworzyć model z istniejącej bazy danych

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Można określić tabel w celu wygenerowania jednostki dla, dodając `-Tables` argument polecenia powyżej. Na przykład `-Tables Blog,Post`.

Proces odtwarzania utworzony klas jednostek (`Blog` i `Post`) i pochodnej kontekstu (`BloggingContext`) na podstawie schematu istniejącej bazy danych.

Klasy jednostki są proste obiekty języka C#, które reprezentują będziesz zapytań i zapisywanie danych. Poniżej przedstawiono `Blog` i `Post` klas jednostek:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Aby włączyć ładowanie z opóźnieniem, należy wybrać właściwości nawigacji `virtual` (Blog.Post i Post.Blog).

Kontekst reprezentuje sesję z bazą danych. Zawiera metody, które służy do wykonywania zapytań i Zapisz wystąpień klas jednostek.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Użyj modelu

Można teraz używać modelu przeprowadzić dostępu do danych.

* Otwórz *Program.cs*

* Zastąp zawartość pliku następującym kodem

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Debuguj > Uruchom bez debugowania

  Zobaczysz, że blogami jest zapisywany w bazie danych, a następnie szczegółowe informacje o wszystkich blogów są drukowane w konsoli.

  ![obraz](_static/output-existing-db.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [EF Core w programie .NET Framework za pomocą nowej bazy danych](xref:core/get-started/full-dotnet/new-db)
* [EF Core na platformie .NET Core za pomocą nowej bazy danych — bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.
