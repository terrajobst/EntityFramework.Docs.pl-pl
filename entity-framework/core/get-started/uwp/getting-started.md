---
title: "Wprowadzenie do platformy uniwersalnej systemu Windows — nowej bazy danych — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Wprowadzenie do podstawowych EF na platformę uniwersalną systemu Windows (UWP) z nową bazę danych

> [!NOTE]
> W tym samouczku używana Core EF 2.0.1 (wydane z platformy ASP.NET Core i .NET Core SDK 2.0.3). EF Core 2.0.0 brakuje niektórych ważnych poprawki wymagane do dobrej obsługi platformy uniwersalnej systemu Windows.

W tym przewodniku będzie tworzenia aplikacji systemu Windows platformy Uniwersalnej, która wykonuje dostęp do podstawowych danych lokalnej bazy danych SQLite używający narzędzia Entity Framework.

> [!IMPORTANT]
> **Należy rozważyć unikanie typy anonimowe w zapytaniach LINQ na platformy uniwersalnej systemu Windows**. Wdrażanie aplikacji do sklepu z aplikacjami platformy uniwersalnej systemu Windows wymaga aplikacji do skompilowania z platformą .NET Native. Zapytania z typy anonimowe ma pogorszenie wydajności na platformie .NET Native.

> [!TIP]
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) w witrynie GitHub.

## <a name="prerequisites"></a>Wstępnie wymagane składniki

Poniższe elementy są wymagane w tym przewodniku:

* [Windows 10 twórców spadku zaktualizować](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [Oprogramowanie .NET core 2.0.0 SDK](https://www.microsoft.com/net/core) lub nowszym.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15,4 lub nowszym z **platformy uniwersalnej systemu Windows dla deweloperów** obciążenia.

## <a name="create-a-new-model-project"></a>Utwórz nowy projekt modelu

> [!WARNING]
> Ze względu na ograniczenia w narzędziach .NET Core sposób interakcji z projektów uniwersalnych systemu Windows, które modelu musi być umieszczona w projekcie aplikacje platformy UWP, aby można było uruchamiać polecenia migracji w konsoli Menedżera pakietów

* Otwórz program Visual Studio

* Plik > Nowy > Projekt...

* Z menu po lewej stronie wybierz Szablony > Visual C#

* Wybierz **biblioteki klas (.NET Standard)** szablonu projektu

* Nadaj nazwę projektu, a następnie kliknij przycisk **OK**

## <a name="install-entity-framework"></a>Instalowanie programu Entity Framework

Aby użyć EF podstawowe, należy zainstalować pakiet dla powszechne bazy danych, który ma być docelowa. W tym przewodniku zastosowano SQLite. Lista dostępnych dostawców [dostawcy bazy danych](../../providers/index.md).

* Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów

* Uruchom`Install-Package Microsoft.EntityFrameworkCore.Sqlite`

W dalszej części tego przewodnika również użyjemy narzędzi Framework niektóre jednostki do obsługi bazy danych. Dlatego zostanie zainstalowany pakiet narzędzi również.

* Uruchom`Install-Package Microsoft.EntityFrameworkCore.Tools`

* Przeprowadź edycję pliku .csproj i Zastąp `<TargetFramework>netstandard2.0</TargetFramework>` z`<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>Tworzenie modelu

Teraz nadszedł czas do definiowania klas kontekstu i jednostek, wchodzące w skład modelu.

* Projekt > Dodaj klasę...

* Wprowadź *Model.cs* jako nazwy i kliknij przycisk **OK**

* Zastąp zawartość pliku następującym kodem

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Utwórz nowy projekt platformy uniwersalnej systemu Windows

* Otwórz program Visual Studio

* Plik > Nowy > Projekt...

* Z menu po lewej stronie wybierz Szablony > Visual C# > uniwersalnych systemu Windows

* Wybierz **pusta aplikacja (uniwersalna systemu Windows)** szablonu projektu

* Nadaj nazwę projektu, a następnie kliknij przycisk **OK**

* Ustaw docelowy i co najmniej wersji do co najmniej`Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>Tworzenie bazy danych

Teraz, gdy masz modelu można użyć migracji tworzenia bazy danych dla Ciebie.

* Pakiet NuGet –> Narzędzia Menedżera –> Konsola Menedżera pakietów

* Wybierz projekt z modelem jako domyślny projekt i ustaw go jako projekt startowy

* Uruchom `Add-Migration MyFirstMigration` Aby utworzyć szkielet migracji do utworzenia wstępnego zestawu tabel dla modelu.

Ponieważ chcemy bazy danych ma zostać utworzony na urządzeniu, która aplikacja jest uruchamiana na dodamy kodu, zastosuj wszelkie oczekujące migracje do lokalnej bazy danych podczas uruchamiania aplikacji. Aplikacja jest uruchamiana, po raz pierwszy to zajmie się tworzenie firmie Microsoft w lokalnej bazie danych.

* Kliknij prawym przyciskiem myszy **App.xaml** w **Eksploratora rozwiązań** i wybierz **widoku kodu**

* Dodaj wyróżnione using na początku pliku

* Dodaj wyróżniony kod, aby Zastosuj wszelkie oczekujące migracje

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Jeśli wprowadzisz zmiany w przyszłości do modelu, możesz użyć `Add-Migration` polecenie, aby utworzyć szkielet nowe migracji, aby zastosować odpowiednie zmiany w bazie danych. Wszelkie oczekujące migracje zostaną zastosowane do lokalnej bazy danych na każdym urządzeniu, podczas uruchamiania aplikacji.
>
>Używa EF `__EFMigrationsHistory` tabeli w bazie danych, aby śledzić migracji, które zostały już zastosowane do bazy danych.

## <a name="use-your-model"></a>Użyj modelu

Można teraz używać modelu do uzyskania dostępu do danych.

* Otwórz *MainPage.xaml*

* Dodawanie obsługi obciążenia strony i interfejsu użytkownika zawartości wskazanych poniżej

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Teraz dodamy kod, aby okablować interfejsu użytkownika z bazy danych

* Kliknij prawym przyciskiem myszy **MainPage.xaml** w **Eksploratora rozwiązań** i wybierz **widoku kodu**

* Dodaj wyróżniony kod z poniższej listy

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Można teraz uruchomić aplikację, aby zobaczyć ją w akcji.

* Debuguj > Uruchom bez debugowania

* Aplikacja tworzenie i uruchamianie

* Wprowadź adres URL, a następnie kliknij przycisk **Dodaj** przycisku

![obraz](_static/create.png)

![obraz](_static/list.png)

## <a name="next-steps"></a>Następne kroki

> [!TIP]
> `SaveChanges()`można poprawić wydajność dzięki implementacji [ `INotifyPropertyChanged` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [ `INotifyPropertyChanging` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [ `INotifyCollectionChanged` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) w Twojej typów jednostek i przy użyciu `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

Tada! Masz teraz prostej aplikacji platformy uniwersalnej systemu Windows z programu Entity Framework.

Zapoznaj się z innymi artykułami w tej dokumentacji, aby dowiedzieć się więcej na temat funkcji programu Entity Framework.
