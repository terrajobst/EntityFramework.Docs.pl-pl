---
title: Wprowadzenie do platformy .NET Framework — Nowa baza danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614431"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Wprowadzenie do programu EF Core w programie .NET Framework za pomocą nowej bazy danych

W tym samouczku utworzysz aplikację konsolową, która wykonuje dostęp do podstawowych danych względem bazy danych programu Microsoft SQL Server przy użyciu platformy Entity Framework. Migracje umożliwia tworzenie bazy danych z modelu.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Tworzenie nowego projektu

* Otwórz program Visual Studio 2017

* **Plik > Nowy > Projekt...**

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Desktop**

* Wybierz **Aplikacja konsoli (.NET Framework)** szablonu projektu

* Upewnij się, że projekt jest ukierunkowany **platformy .NET Framework 4.6.1** lub nowszej

* Nadaj projektowi nazwę *ConsoleApp.NewDb* i kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. Ten samouczek używa programu SQL Server. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

W dalszej części tego samouczka użyjesz niektórych narzędzi Entity Framework Tools do obsługi bazy danych. Więc Zainstaluj również pakiet narzędzi.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Tworzenie modelu

Teraz nadszedł czas, do definiowania klas kontekstu i jednostek, które tworzą model.

* **Projekt > Dodaj klasę...**

* Wprowadź *Model.cs* jako nazwę i kliknij przycisk **OK**

* Zastąp zawartość pliku następującym kodem

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> W rzeczywistej aplikacji można będzie umieścić każda klasa w oddzielnym pliku, a parametry połączenia w zmiennej środowisku lub pliku konfiguracji. Dla uproszczenia wszystko jest w pliku pojedynczego kodu na potrzeby tego samouczka.

## <a name="create-the-database"></a>Tworzenie bazy danych

Teraz, gdy model, można użyć migracje utworzyć bazę danych.

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Uruchom `Add-Migration InitialCreate` do tworzenia szkieletu migracji, aby utworzyć początkowy zestaw tabel dla modelu.

* Uruchom `Update-Database` zastosować nową migrację do bazy danych. Ponieważ baza danych nie istnieje, zostanie utworzony, przed zastosowaniem migracji.

> [!TIP]  
> Jeśli wprowadzisz zmiany w modelu, można użyć `Add-Migration` polecenia do tworzenia szkieletu nowej migracji w celu sprawdzenia odpowiedni schemat zmienia się z bazą danych. Po zaznaczeniu tej opcji utworzony szkielet kodu (i wprowadzone wymagane zmiany), można użyć `Update-Database` polecenie, aby zastosować zmiany do bazy danych.
>
> Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.

## <a name="use-the-model"></a>Użyj modelu

Można teraz używać modelu przeprowadzić dostępu do danych.

* Otwórz *Program.cs*

* Zastąp zawartość pliku następującym kodem

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Debuguj > Uruchom bez debugowania**

  Zobaczysz, że blogami jest zapisywany w bazie danych, a następnie szczegółowe informacje o wszystkich blogów są drukowane w konsoli.

  ![obraz](_static/output-new-db.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [EF Core w programie .NET Framework przy użyciu istniejącej bazy danych](xref:core/get-started/full-dotnet/existing-db)
* [EF Core na platformie .NET Core za pomocą nowej bazy danych — bazy danych SQLite](xref:core/get-started/netcore/new-db-sqlite) — samouczek programu EF konsoli dla wielu platform.
