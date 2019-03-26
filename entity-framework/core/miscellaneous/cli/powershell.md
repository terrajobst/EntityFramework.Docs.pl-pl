---
title: EF Core informacje dotyczące narzędzi (Konsola Menedżera pakietów) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419773"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Entity Framework Core odnoszą się narzędzia — Konsola Menedżera pakietów w programie Visual Studio

Narzędzia konsoli Menedżera pakietów (PMC) dla platformy Entity Framework Core wykonywać zadania podczas projektowania. Na przykład można utworzyć [migracje](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), zastosuj migracje i wygenerować kod dla modelu, w oparciu o istniejącą bazę danych. Polecenia uruchamiane wewnątrz programu Visual Studio przy użyciu [Konsola Menedżera pakietów](/nuget/tools/package-manager-console). Te narzędzia działają w projektach .NET Core i .NET Framework.

Jeśli nie używasz programu Visual Studio, firma Microsoft zaleca [narzędzi wiersza polecenia programu EF Core](dotnet.md) zamiast tego. Narzędzia interfejsu wiersza polecenia są wieloplatformowe i wykonywania w wierszu polecenia.

## <a name="installing-the-tools"></a>Instalowanie narzędzi

Procedury instalowania i aktualizowania narzędzia różnią się między platformą ASP.NET Core 2.1 + i wcześniejszych wersji lub innych typów projektów.

### <a name="aspnet-core-version-21-and-later"></a>Platforma ASP.NET Core w wersji 2.1 i nowsze

Narzędzia są automatycznie uwzględniane w projektach programu ASP.NET Core 2.1 +, ponieważ `Microsoft.EntityFrameworkCore.Tools` pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app).

W związku z tym nie trzeba nic robić, aby zainstalować narzędzia, ale trzeba:
* Przywróć pakiety przed użyciem narzędzia w nowym projekcie.
* Zainstaluj pakiet, aby zaktualizować narzędzia do nowszej wersji.

Aby upewnić się, że otrzymujesz najnowszej wersji narzędzia, firma Microsoft zaleca również wykonać poniższe czynności:

* Edytuj swoje *.csproj* pliku i Dodaj wiersz, określając najnowszą wersję [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) pakietu. Na przykład *.csproj* plik może zawierać `ItemGroup` , wygląda następująco:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aktualizacja narzędzi, gdy zostanie wyświetlony komunikat jak w poniższym przykładzie:

> Wersja narzędzi programu EF Core "2.1.1-rtm-30846" jest starsza niż w przypadku środowiska uruchomieniowego "2.1.3-rtm-32065". Aktualizacja narzędzi do najnowszych funkcji i poprawek błędów.

Aby zaktualizować narzędzia:
* Zainstaluj najnowszy zestaw .NET Core SDK.
* Aktualizacja programu Visual Studio do najnowszej wersji.
* Edytuj *.csproj* plik zawiera odwołania do pakietu do najnowszego pakietu narzędzi, jak pokazano wcześniej.

### <a name="other-versions-and-project-types"></a>Inne wersje i typy projektów

Instalowanie narzędzi Konsola Menedżera pakietów, uruchamiając następujące polecenie w **Konsola Menedżera pakietów**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Aktualizacja narzędzi, uruchamiając następujące polecenie w **Konsola Menedżera pakietów**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Weryfikowanie instalacji

Sprawdź, czy narzędzia są zainstalowane, uruchamiając następujące polecenie:

``` powershell
Get-Help about_EntityFrameworkCore
```

Dane wyjściowe wyglądają następująco (program nie nakazuje możesz wersję narzędzia używasz):

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Przy użyciu narzędzi

Przed rozpoczęciem korzystania z narzędzi:
* Należy zrozumieć różnicę między docelowej i uruchamianie projektu.
* Dowiedz się, jak używać narzędzi z biblioteki klas .NET Standard.
* Dla projektów ASP.NET Core należy ustawić środowisko.

### <a name="target-and-startup-project"></a>Element docelowy i uruchamianie projektu

Polecenia można znaleźć *projektu* i *projekt startowy*.

* *Projektu* jest także znana jako *projekt docelowy* ponieważ jest za której polecenia Dodawanie lub usuwanie plików. Domyślnie **projekt domyślny** wybranego w **Konsola Menedżera pakietów** jest projekt docelowy. Należy określić inny projekt jako projekt docelowy za pomocą <nobr> `--project` </nobr> opcji.

* *Projekt startowy* jest tą, która narzędzia skompilować i uruchomić. Narzędzia konieczne wykonanie kodu aplikacji w czasie projektowania, aby uzyskać informacje na temat projektu, takie jak parametry połączenia bazy danych i Konfiguracja modelu. Domyślnie **projekt startowy** w **Eksploratora rozwiązań** jest projektem startowym. Należy określić inny projekt jako projekt startowy, za pomocą <nobr> `--startup-project` </nobr> opcji.

Projekt startowy i projekt docelowy często są tym samym projekcie. Jest to typowy scenariusz, w którym są one oddzielnych projektów, gdy:

* EF Core klasy kontekstu i jednostki są w bibliotece klas .NET Core.
* Aplikacją sieci web lub aplikacji konsolowej .NET Core odwołuje się do biblioteki klas.

Istnieje również możliwość [umieść kod migracji w bibliotece klas, niezależnie od kontekstu programu EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Innych platform docelowych

Narzędzia Konsola Menedżera pakietów pracować z projektami .NET Core lub .NET Framework. Aplikacje, które mają modelu platformy EF Core w bibliotece klas programu .NET Standard może nie mieć platformy .NET Core lub .NET Framework projektu. Na przykład dotyczy to aplikacji platformy Xamarin i platformy uniwersalnej Windows. W takich przypadkach można utworzyć platformy .NET Core lub .NET Framework projekt aplikacji konsoli którego jedynym celem jest do działania jako projekt startowy dla narzędzi. Projekt może być fikcyjnego projektu bez rzeczywistego kodu &mdash; jest wymagana tylko do zapewnienia celu narzędzi.

Dlaczego jest projektem fikcyjnego wymagane? Jak wspomniano wcześniej, narzędzia konieczne wykonanie kodu aplikacji w czasie projektowania. Aby to zrobić, muszą one korzystania ze środowiska uruchomieniowego .NET Core lub .NET Framework. Jeśli model programu EF Core jest w projekcie, który jest przeznaczony dla platformy .NET Core lub .NET Framework, narzędzia programu EF Core "pożyczać" środowisko uruchomieniowe z projektu. Nie można wykonać, jeśli model programu EF Core jest w bibliotece klas programu .NET Standard. .NET Standard nie jest rzeczywistą implementację .NET; jest określenie zestawu interfejsów API, który musi obsługiwać implementacje platformy .NET. W związku z tym .NET Standard nie jest wystarczająca dla narzędzi programu EF Core do wykonywania kodu aplikacji. Projekt zastępczy, utworzone jako projekt startowy zawiera platformy konkretnego celu, w którym narzędzia można załadować biblioteki klas .NET Standard.

### <a name="aspnet-core-environment"></a>Środowiska ASP.NET Core

Aby określić środowisko dla projektów ASP.NET Core, ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem polecenia.

## <a name="common-parameters"></a>Wspólne parametry

W poniższej tabeli przedstawiono parametry, które są wspólne dla wszystkich poleceń programu EF Core:

| Parametr                 | Opis                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Kontekstu \<ciąg >        | `DbContext` Klasy. Nazwa klasy tylko lub w pełni kwalifikowana za pomocą przestrzeni nazw.  Jeśli ten parametr zostanie pominięty, programem EF Core umożliwia znalezienie klasy kontekstu. W przypadku wielu klas kontekstu, ten parametr jest wymagany. |
| -Projekt \<ciąg >        | Projekt docelowy. Jeśli ten parametr zostanie pominięty, **projekt domyślny** dla **Konsola Menedżera pakietów** służy jako projekt docelowy.                                                                             |
| Projekt startowy - \<ciąg > | Projekt startowy. Jeśli ten parametr zostanie pominięty, **projekt startowy** w **właściwości rozwiązania** służy jako projekt docelowy.                                                                                 |
| -Verbose                  | Wyświetlić pełne dane wyjściowe.                                                                                                                                                                                                 |

Aby wyświetlić Pomoc dotyczącą polecenia, użyj programu PowerShell `Get-Help` polecenia.

> [!TIP]
> Parametry kontekstu, projekt i projekt startowy obsługują rozszerzenia karty.

## <a name="add-migration"></a>Dodaj migracji

Dodaje nową migrację.

Parametry:

| Parametr                         | Opis                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>— Nazwa \<ciąg ><nobr>       | Nazwa migracji. To jest parametr pozycyjne i jest wymagana.                                              |
| <nobr>-OutputDir \<ciąg ></nobr> | Katalog (i podrzędnej przestrzeni nazw) do użycia. Ścieżki są względne wobec katalogu docelowego projektu. Wartość domyślna to "Migracja". |

## <a name="drop-database"></a>Baza danych listy

Porzuca bazy danych.

Parametry:

| Parametr | Opis                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej. |

## <a name="get-dbcontext"></a>Get-DbContext

Pobiera informacje `DbContext` typu.

## <a name="remove-migration"></a>Usuń migrację

Usuwa ostatniej migracji (wycofanie zmian w kodzie, które zostały przygotowane do migracji).

Parametry:

| Parametr | Opis                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Przywróć migracji (wycofać zmiany, które zostały zastosowane do bazy danych). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Generuje kod dla `DbContext` i typy jednostek bazy danych. Aby `Scaffold-DbContext` można wygenerować typu jednostki, w tabeli bazy danych musi mieć klucz podstawowy.

Parametry:

| Parametr                          | Opis                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection \<ciąg ></nobr> | Parametry połączenia z bazą danych. W przypadku projektów ASP.NET Core 2.x wartość może być *nazwa =\<nazwa parametrów połączenia >*. W takim przypadku nazwa pochodzi od źródła konfiguracji, które są skonfigurowane dla projektu. To jest parametr pozycyjne i jest wymagana. |
| <nobr>-Provider \<String></nobr>   | Dostawca do użycia. Zazwyczaj jest to nazwa pakietu NuGet, na przykład: `Microsoft.EntityFrameworkCore.SqlServer`. To jest parametr pozycyjne i jest wymagana.                                                                                           |
| -OutputDir \<ciąg >               | Katalog, który można umieścić pliki w. Ścieżki są względne wobec katalogu projektu.                                                                                                                                                                                             |
| -ContextDir \<ciąg >              | Katalog, aby umieścić `DbContext` w pliku. Ścieżki są względne wobec katalogu projektu.                                                                                                                                                                              |
| -Kontekstu \<ciąg >                 | Nazwa `DbContext` klasy do wygenerowania.                                                                                                                                                                                                                          |
| -Schematów \<String [] >               | Schematów tabel w celu wygenerowania typów jednostek dla. Jeśli ten parametr zostanie pominięty, uwzględniono wszystkich schematów.                                                                                                                                                             |
| -Tabele \<String [] >                | Tabele, aby wygenerować typy jednostek dla. Jeżeli ten parametr zostanie pominięty, uwzględniane są wszystkie tabele.                                                                                                                                                                         |
| -DataAnnotations                   | Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów. Jeżeli pominięto ten parametr jest używany tylko interfejsu API fluent.                                                                                                                                                      |
| -UseDatabaseNames                  | Użyj nazwy tabel i kolumn dokładnie tak, jak pojawiają się w bazie danych. Jeśli ten parametr zostanie pominięty, nazwy baz danych zostały zmienione, aby ściślej odpowiadają Konwencji stylistycznych nazwy języka C#.                                                                                       |
| -Force                             | Nadpisz istniejące pliki.                                                                                                                                                                                                                                               |

Przykład:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Przykład szkielety mechanizmów tylko wybrane tabele i tworzy kontekst w oddzielnym folderze z określoną nazwą:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Skrypt migracji

Generuje skrypt SQL, który dotyczy wszystkich zmian z jedną migrację wybranych innej wybranej migracji.

Parametry:

| Parametr                | Opis                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-From* \<ciąg >        | Począwszy od migracji. Migracje mogą być określane według nazwy lub identyfikatora. Numer 0 jest przypadkiem szczególnym, oznacza to *przed migracją pierwszy*. Wartość domyślna to 0.                                                              |
| *— Do* \<ciąg >          | Końcowy migracji. Domyślnie do ostatniego migracji.                                                                                                                                                                      |
| <nobr>-Idempotentne</nobr> | Generowanie skryptu, który może służyć w bazie danych w każdej migracji.                                                                                                                                                         |
| -Dane wyjściowe \<ciąg >        | Plik można zapisać wynik. Jeśli ten parametr zostanie pominięty, plik jest tworzony z użyciem nazwy wygenerowanej w tym samym folderze, ponieważ pliki środowiska uruchomieniowego aplikacji są tworzone, na przykład: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> To, z danych wyjściowych z obsługą i parametrów rozszerzenia karty.

Poniższy przykład tworzy skrypt w przypadku migracji InitialCreate przy użyciu nazwy migracji.

```powershell
Script-Migration -To InitialCreate
```

Poniższy przykład tworzy skrypt wszystkich migracji po migracji InitialCreate przy użyciu identyfikatora migracji.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Update-Database

Aktualizuje bazę danych do ostatniej migracji lub określony migracji.

| Parametr                           | Opis                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*— Migracja* \<ciąg ></nobr> | Migracja docelowego. Migracje mogą być określane według nazwy lub identyfikatora. Numer 0 jest przypadkiem szczególnym, oznacza to *przed migracją pierwszy* i powoduje, że wszystkich migracji, które można przywrócić. Jeśli migracja nie zostanie określony, polecenie domyślne ostatnio migracji. |

> [!TIP]
> Parametr migracji obsługuje rozszerzenia karty.

Poniższy przykład przywraca wszystkich migracji.

```powershell
Update-Database -Migration 0
```

Poniższe przykłady aktualizują bazę danych do określonego migracji. Pierwszy używa nazwy migracji, a drugi używa Identyfikatora migracji:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Migracje](xref:core/managing-schemas/migrations/index)
* [Odtwarzanie](xref:core/managing-schemas/scaffolding)
