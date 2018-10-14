---
title: Wprowadzenie do platformy UWP — Nowa baza danych — EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315623"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Wprowadzenie do programu EF Core na platformie Universal Windows (UWP) przy użyciu nowej bazy danych

W tym samouczku utworzysz aplikację platformy uniwersalnej Windows (UWP), która wykonuje dostęp do podstawowych danych lokalnej bazy danych SQLite przy użyciu platformy Entity Framework Core.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Wymagania wstępne

* [Windows 10 Fall Creators Update (10.0; Kompilacja 16299) lub nowszym](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/) z **Universal Windows Platform Development** obciążenia.

* [Zestaw SDK programu .NET core 2.1 lub nowszej](https://www.microsoft.com/net/core) lub nowszej.

> [!IMPORTANT]
> Ten samouczek używa platformy Entity Framework Core [migracje](xref:core/managing-schemas/migrations/index) poleceń do tworzenia i aktualizowania schematu bazy danych.
> Te polecenia nie działają bezpośrednio z projektów platformy UWP.
> Z tego powodu modelu danych aplikacji znajduje się w projekcie biblioteki udostępnionej, a oddzielną aplikację konsoli .NET Core jest używany do uruchamiania polecenia.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Utwórz projekt biblioteki do przechowywania modelu danych

* Otwórz program Visual Studio

* **Plik > Nowy > Projekt**

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Standard**.

* Wybierz **biblioteki klas (.NET Standard)** szablonu.

* Nadaj projektowi nazwę *Blogging.Model*.

* Nazwij rozwiązanie *do obsługi blogów*.

* Kliknij przycisk **OK**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Zainstaluj środowisko uruchomieniowe programu Entity Framework Core w projekcie modelu danych

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. Ten samouczek używa bazy danych SQLite. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**.

* Upewnij się, że projekt biblioteki *Blogging.Model* jest wybrany jako **domyślny projekt** w konsoli Menedżera pakietów.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

## <a name="create-the-data-model"></a>Tworzenie modelu danych

Teraz nadszedł czas, aby zdefiniować *DbContext* i klas jednostek, które tworzą model.

* Usuń *Class1.cs*.

* Tworzenie *Model.cs* następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Utwórz nowy projekt konsoli, aby uruchamiać polecenia migracji

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.

* Wybierz **Aplikacja konsoli (.NET Core)** szablonu projektu.

* Nadaj projektowi nazwę *Blogging.Migrations.Startup*i kliknij przycisk **OK**.

* Dodaj odwołanie do projektu z *Blogging.Migrations.Startup* projekt *Blogging.Model* projektu.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Zainstaluj narzędzia Entity Framework Core w projekcie uruchamiania migracji

Aby włączyć polecenia migracji EF Core w konsoli Menedżera pakietów, należy zainstalować pakiet narzędzi programu EF Core w aplikacji konsoli.

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`

## <a name="create-the-initial-migration"></a>Tworzenie początkowej migracji

 Tworzenie początkowej migracji, określenie aplikacji konsoli jako projekt startowy.

* Uruchom `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`

To polecenie scaffolds migracji, który tworzy początkowy zestaw tabel bazy danych dla modelu danych.

## <a name="create-the-uwp-project"></a>Tworzenie projektu platformy uniwersalnej systemu Windows

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Universal**.

* Wybierz **pusta aplikacja (Windows Universal)** szablonu projektu.

* Nadaj projektowi nazwę *Blogging.UWP*i kliknij przycisk **OK**

> [!IMPORTANT]
> Co najmniej równa wersje docelową i minimalną **Windows 10 Fall Creators Update (10.0; kompilacja 16299.0)**.
> Poprzednie wersje systemu Windows 10 nie obsługują .NET Standard 2.0, która jest wymagana przez Entity Framework Core.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Dodaj kod, aby utworzyć bazę danych podczas uruchamiania aplikacji

Ponieważ baza danych ma zostać utworzony na urządzeniu, która jest uruchamiana aplikacja, należy dodać kod Zastosuj wszelkie oczekujące migracji w lokalnej bazie danych podczas uruchamiania aplikacji. Przy pierwszym uruchomieniu aplikacji, to zajmie się tworzenie lokalnej bazy danych.

* Dodaj odwołanie do projektu z *Blogging.UWP* projekt *Blogging.Model* projektu.

* Otwórz *App.xaml.cs*.

* Dodaj wyróżniony kod, aby zastosować wszelkie oczekujące migracji.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> W przypadku zmiany modelu użycia `Add-Migration` polecenia do tworzenia szkieletu nową migrację do zastosowania odpowiednich zmian w bazie danych. Wszystkie oczekujące migracje zostaną zastosowane do lokalnej bazy danych na każdym urządzeniu, podczas uruchamiania aplikacji.
>
>Używa programu EF Core `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.

## <a name="use-the-data-model"></a>Przy użyciu modelu danych

Można teraz używać programu EF Core przeprowadzić dostępu do danych.

* Otwórz *MainPage.xaml*.

* Dodawanie obsługi ładowania strony i UI zawartości, które przedstawiono poniżej

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Teraz Dodaj kod, aby Podłączanie do interfejsu użytkownika z bazą danych

* Otwórz *MainPage.xaml.cs*.

* Dodaj wyróżniony kod z poniższej listy:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Teraz można uruchomić aplikacji, aby zobaczyć go w działaniu.

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Blogging.UWP* projektu, a następnie wybierz pozycję **Wdróż**.

* Ustaw *Blogging.UWP* jako projekt startowy.

* **Debuguj > Uruchom bez debugowania**

  Aplikacja tworzy i uruchamia.

* Wprowadź adres URL, a następnie kliknij przycisk **Dodaj** przycisku

  ![obraz](_static/create.png)

  ![obraz](_static/list.png)

  Tada! Masz teraz platformy Entity Framework Core uruchomioną prostą aplikacją platformy uniwersalnej systemu Windows.

## <a name="next-steps"></a>Następne kroki

Uzyskać zgodności i wydajności, którą należy wiedzieć podczas korzystania z programu EF Core przy użyciu platformy uniwersalnej systemu Windows, zobacz [implementacji platformy .NET obsługiwanych przez platformę EF Core](../../platforms/index.md#universal-windows-platform).

Zapoznaj się z innymi artykułami w tej dokumentacji, aby dowiedzieć się więcej na temat funkcji platformy Entity Framework Core.
