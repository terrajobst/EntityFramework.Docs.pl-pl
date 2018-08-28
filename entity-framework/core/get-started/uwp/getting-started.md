---
title: Wprowadzenie do platformy UWP — Nowa baza danych — EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996913"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Wprowadzenie do programu EF Core na platformie Universal Windows (UWP) przy użyciu nowej bazy danych

W tym samouczku utworzysz aplikację platformy uniwersalnej Windows (UWP), która wykonuje dostęp do podstawowych danych lokalnej bazy danych SQLite przy użyciu platformy Entity Framework Core.

[Wyświetlanie przykładowych w tym artykule w witrynie GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Wymagania wstępne

* [Windows 10 Fall Creators Update (10.0; Kompilacja 16299) lub nowszym](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 w wersji 15.7 lub nowszej](https://www.visualstudio.com/downloads/) z **Universal Windows Platform Development** obciążenia.

* [Zestaw SDK programu .NET core 2.1 lub nowszej](https://www.microsoft.com/net/core) lub nowszej.

## <a name="create-a-model-project"></a>Tworzenie projektu modelu

> [!IMPORTANT]
> Ze względu na ograniczenia w taki sposób, narzędzia platformy .NET Core wchodzą w interakcję z projektów platformy UWP modelu musi być umieszczona w projekcie bez platformy UWP, aby można było uruchamiać polecenia migracji w **Konsola Menedżera pakietów** (PMC)

* Otwórz program Visual Studio

* **Plik > Nowy > Projekt**

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Standard**.

* Wybierz **biblioteki klas (.NET Standard)** szablonu.

* Nadaj projektowi nazwę *Blogging.Model*.

* Nazwij rozwiązanie *do obsługi blogów*.

* Kliknij przycisk **OK**.

## <a name="install-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

Aby korzystać z programu EF Core, należy zainstalować pakiet dla dostawców bazy danych, który ma pod kątem. Ten samouczek używa bazy danych SQLite. Aby uzyskać listę dostępnych dostawców zobacz [dostawcy baz danych](../../providers/index.md).

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

W dalszej części tego samouczka używana będzie niektóre narzędzia Entity Framework Core do obsługi bazy danych. Więc Zainstaluj również pakiet narzędzi.

* Uruchom `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Tworzenie modelu

Teraz nadszedł czas, do definiowania klas kontekstu i jednostek, które tworzą model.

* Usuń *Class1.cs*.

* Tworzenie *Model.cs* następującym kodem:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Utwórz nowy projekt platformy uniwersalnej systemu Windows

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > Windows Universal**.

* Wybierz **pusta aplikacja (Windows Universal)** szablonu projektu.

* Nadaj projektowi nazwę *Blogging.UWP*i kliknij przycisk **OK**

* Co najmniej równa wersje docelową i minimalną **Windows 10 Fall Creators Update (10.0; kompilacja 16299.0)**.

## <a name="create-the-initial-migration"></a>Tworzenie początkowej migracji

Teraz, gdy model, należy skonfigurować aplikację do tworzenia bazy danych przy pierwszym uruchomieniu. W tej sekcji opisano tworzenie początkowej migracji. W poniższej sekcji dodasz kod, który ma zastosowanie tej migracji, po uruchomieniu aplikacji.

Narzędzia migracji wymagają projektu startowego bez platformy UWP, dzięki czemu można tworzyć najpierw.

* W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj > Nowy projekt**.

* Z menu po lewej stronie wybierz **zainstalowane > Visual C# > .NET Core**.

* Wybierz **Aplikacja konsoli (.NET Core)** szablonu projektu.

* Nadaj projektowi nazwę *Blogging.Migrations.Startup*i kliknij przycisk **OK**.

* Dodaj odwołanie do projektu z *Blogging.Migrations.Startup* projekt *Blogging.Model* projektu.

Teraz możesz utworzyć początkowej migracji.

* **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Wybierz *Blogging.Model* projektu jako **projekt domyślny**.

* W **Eksploratora rozwiązań**ustaw *Blogging.Migrations.Startup* projekt jako projekt startowy.

* Uruchom `Add-Migration InitialCreate`.

  To polecenie scaffolds migracji, który tworzy początkowego zestawu tabel dla modelu.

## <a name="create-the-database-on-app-startup"></a>Tworzenie bazy danych podczas uruchamiania aplikacji

Ponieważ baza danych ma zostać utworzony na urządzeniu, która jest uruchamiana aplikacja, należy dodać kod Zastosuj wszelkie oczekujące migracji w lokalnej bazie danych podczas uruchamiania aplikacji. Przy pierwszym uruchomieniu aplikacji, to zajmie się tworzenie lokalnej bazy danych.

* Dodaj odwołanie do projektu z *Blogging.UWP* projekt *Blogging.Model* projektu.

* Otwórz *App.xaml.cs*.

* Dodaj wyróżniony kod, aby zastosować wszelkie oczekujące migracji.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> W przypadku zmiany modelu użycia `Add-Migration` polecenia do tworzenia szkieletu nową migrację do zastosowania odpowiednich zmian w bazie danych. Wszystkie oczekujące migracje zostaną zastosowane do lokalnej bazy danych na każdym urządzeniu, podczas uruchamiania aplikacji.
>
>Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracje, które zostały już zastosowane do bazy danych.

## <a name="use-the-model"></a>Użyj modelu

Można teraz używać modelu przeprowadzić dostępu do danych.

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
