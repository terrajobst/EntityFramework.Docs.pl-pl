---
title: EF Core informacje dotyczące narzędzi (interfejs wiersza polecenia platformy .NET) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 05c5f89fc79556e72a7e629c147aa817fe7d1a6b
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286466"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Entity Framework Core odnoszą się narzędzia — interfejs wiersza polecenia platformy .NET

Narzędzia interfejsu wiersza polecenia (CLI) dla platformy Entity Framework Core wykonywać zadania podczas projektowania. Na przykład można utworzyć [migracje](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), zastosuj migracje i wygenerować kod dla modelu, w oparciu o istniejącą bazę danych. Polecenia są rozszerzenie dla wielu platform [dotnet](/dotnet/core/tools) polecenia, który jest częścią programu [zestawu .NET Core SDK](https://www.microsoft.com/net/core). Te narzędzia działają w projektach .NET Core.

Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia Konsola Menedżera pakietów](powershell.md) zamiast tego:
* Działają automatycznie z bieżącego projektu wybranego w **Konsola Menedżera pakietów** bez konieczności ręcznie Przełącz katalogi.
* One automatycznie otwarte pliki wygenerowane za pomocą polecenia, po zakończeniu polecenia.

## <a name="installing-the-tools"></a>Instalowanie narzędzi

Procedura instalacji zależy od typu projektu, a wersja:

* EF Core 3.x
* Platforma ASP.NET Core w wersji 2.1 i nowsze
* EF Core 2.x
* EF Core 1.x

### <a name="ef-core-3x"></a>EF Core 3.x

* `dotnet ef` musi być zainstalowany jako narzędzie globalnych lub lokalnych. Większość programistów zainstaluje `dotnet ef` jako globalne narzędzia, za pomocą następującego polecenia:

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

  Można również użyć `dotnet ef` jako lokalne narzędzie. Aby użyć go jako lokalne narzędzie, należy przywrócić zależności projektu, który deklaruje ją jako zależność narzędzia przy użyciu [plik manifestu narzędzia](https://github.com/dotnet/cli/issues/10288).

* Zainstaluj [platformy .NET Core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)). Zestaw SDK został zainstalowany, nawet jeśli masz najnowszą wersję programu Visual Studio.

* Zainstaluj najnowszą wersję `Microsoft.EntityFrameworkCore.Design` pakietu.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>Platforma ASP.NET Core 2.1 +

* Zainstaluj bieżącą [zestawu .NET Core SDK](https://www.microsoft.com/net/download/core). Zestaw SDK został zainstalowany, nawet jeśli masz najnowszą wersję programu Visual Studio 2017.

  To wszystko, co jest niezbędne dla platformy ASP.NET Core 2.1 +, ponieważ `Microsoft.EntityFrameworkCore.Design` pakietu znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (nie platformy ASP.NET Core)

`dotnet ef` Poleceń są zawarte w zestawie SDK programu .NET Core, ale aby włączyć polecenia należy zainstalować `Microsoft.EntityFrameworkCore.Design` pakietu.

* Zainstaluj bieżącą [zestawu .NET Core SDK](https://www.microsoft.com/net/download/core). Zestaw SDK został zainstalowany, nawet jeśli masz najnowszą wersję programu Visual Studio.

* Zainstaluj najnowszy stabilny `Microsoft.EntityFrameworkCore.Design` pakietu.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Zainstaluj zestaw .NET Core SDK w wersji 2.1.200. Nowsze wersje nie są zgodne z narzędzi interfejsu wiersza polecenia dla programu EF Core 1.0 i 1.1.

* Konfigurowanie aplikacji do użycia 2.1.200 wersja zestawu SDK, modyfikując jego [global.json](/dotnet/core/tools/global-json) pliku. Ten plik zwykle znajduje się w katalogu rozwiązania (po jednym powyżej projektu).

* Edytowanie pliku projektu i dodawanie `Microsoft.EntityFrameworkCore.Tools.DotNet` jako `DotNetCliToolReference` elementu. Na przykład określić najnowszej wersji 1.x: 1.1.6. Zobacz przykład pliku projektu, na końcu tej sekcji.

* Zainstaluj najnowszą wersję 1.x `Microsoft.EntityFrameworkCore.Design` pakietów, na przykład:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Plik projektu za pomocą obu dodać odwołania do pakietu, wygląda mniej więcej tak:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Odwołania do pakietu przy użyciu `PrivateAssets="All"` nie jest widoczne dla projektów odwołujących się do tego projektu. To ograniczenie jest szczególnie przydatne w przypadku pakietów, które zazwyczaj są używane tylko podczas programowania.

### <a name="verify-installation"></a>Weryfikowanie instalacji

Uruchom następujące polecenia, aby zweryfikować poprawne zainstalowanie narzędzi interfejsu wiersza polecenia programu EF Core:

  ``` Console
  dotnet restore
  dotnet ef
  ```

Dane wyjściowe polecenia identyfikuje wersję narzędzia w użyciu:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Przy użyciu narzędzi

Przed rozpoczęciem korzystania z narzędzi, trzeba utworzyć projekt startowy, lub należy ustawić środowisko.

### <a name="target-project-and-startup-project"></a>Projekt docelowy i projekt startowy

Polecenia można znaleźć *projektu* i *projekt startowy*.

* *Projektu* jest także znana jako *projekt docelowy* ponieważ jest za której polecenia Dodawanie lub usuwanie plików. Domyślnie projekt w bieżącym katalogu jest projekt docelowy. Należy określić inny projekt jako projekt docelowy za pomocą <nobr> `--project` </nobr> opcji.

* *Projekt startowy* jest tą, która narzędzia skompilować i uruchomić. Narzędzia konieczne wykonanie kodu aplikacji w czasie projektowania, aby uzyskać informacje na temat projektu, takie jak parametry połączenia bazy danych i Konfiguracja modelu. Domyślnie projekt w bieżącym katalogu jest projekt startowy. Należy określić inny projekt jako projekt startowy, za pomocą <nobr> `--startup-project` </nobr> opcji.

Projekt startowy i projekt docelowy często są tym samym projekcie. Jest to typowy scenariusz, w którym są one oddzielnych projektów, gdy:

* EF Core klasy kontekstu i jednostki są w bibliotece klas .NET Core.
* Aplikacją sieci web lub aplikacji konsolowej .NET Core odwołuje się do biblioteki klas.

Istnieje również możliwość [umieść kod migracji w bibliotece klas, niezależnie od kontekstu programu EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Innych platform docelowych

Narzędzia interfejsu wiersza polecenia pracować z projektami .NET Core i projektów programu .NET Framework. Aplikacje, które mają modelu platformy EF Core w bibliotece klas programu .NET Standard może nie mieć platformy .NET Core lub .NET Framework projektu. Na przykład dotyczy to aplikacji platformy Xamarin i platformy uniwersalnej Windows. W takich przypadkach można utworzyć projekt aplikacji konsoli .NET Core, którego jedynym celem jest do działania jako projekt startowy dla narzędzi. Projekt może być fikcyjnego projektu bez rzeczywistego kodu &mdash; jest wymagana tylko do zapewnienia celu narzędzi.

Dlaczego jest projektem fikcyjnego wymagane? Jak wspomniano wcześniej, narzędzia konieczne wykonanie kodu aplikacji w czasie projektowania. Aby to zrobić, muszą one korzystania ze środowiska uruchomieniowego .NET Core. Jeśli model programu EF Core jest w projekcie, który jest przeznaczony dla platformy .NET Core lub .NET Framework, narzędzia programu EF Core "pożyczać" środowisko uruchomieniowe z projektu. Nie można wykonać, jeśli model programu EF Core jest w bibliotece klas programu .NET Standard. .NET Standard nie jest rzeczywistą implementację .NET; jest określenie zestawu interfejsów API, który musi obsługiwać implementacje platformy .NET. W związku z tym .NET Standard nie jest wystarczająca dla narzędzi programu EF Core do wykonywania kodu aplikacji. Projekt zastępczy, utworzone jako projekt startowy zawiera platformy konkretnego celu, w którym narzędzia można załadować biblioteki klas .NET Standard.

### <a name="aspnet-core-environment"></a>Środowiska ASP.NET Core

Aby określić środowisko dla projektów ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem polecenia.

## <a name="common-options"></a>Typowe opcje

|                   | Opcja                            | Opis                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | Pokaż dane wyjściowe JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | `DbContext` Klasy. Nazwa klasy tylko lub w pełni kwalifikowana za pomocą przestrzeni nazw.  Jeśli ta opcja zostanie pominięty, programem EF Core znajdzie klasy kontekstu. W przypadku wielu klas kontekstu, ta opcja jest wymagana.                                            |
| `-p`              | `--project <PROJECT>`             | Względna ścieżka do folderu projektu do projektu docelowego.  Wartością domyślną jest bieżący folder.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Względna ścieżka do folderu projektu Projekt startowy. Wartością domyślną jest bieżący folder.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | [Moniker platformy docelowej](/dotnet/standard/frameworks#supported-target-framework-versions) dla [platformę docelową](/dotnet/standard/frameworks).  Użyj pliku projektu określa wiele platform docelowych, gdy chcesz wybrać jeden z nich. |
|                   | `--configuration <CONFIGURATION>` | Konfigurację kompilacji, na przykład: `Debug` lub `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | Identyfikator docelowe środowisko uruchomieniowe w celu przywrócenia pakietów dla. Aby uzyskać listę identyfikatorów środowisk uruchomieniowych (RID), zobacz [katalogu RID](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Pokaż informacje pomocy.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Wyświetlić pełne dane wyjściowe.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Nie kolorować danych wyjściowych.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Prefiks danych wyjściowych z poziomu.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>docelowej bazie danych ef DotNet

Porzuca bazy danych.

Opcje:

|                   | Opcja                   | Opis                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Nie Potwierdź.                                           |
|                   | <nobr>`--dry-run`</nobr> | Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej. |

## <a name="dotnet-ef-database-update"></a>Aktualizacja bazy danych programu ef DotNet

Aktualizuje bazę danych do ostatniej migracji lub określony migracji.

Argumenty:

| Argument      | Opis                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Migracja docelowego. Migracje mogą być określane według nazwy lub identyfikatora. Numer 0 jest przypadkiem szczególnym, oznacza to *przed migracją pierwszy* i powoduje, że wszystkich migracji, które można przywrócić. Jeśli migracja nie zostanie określony, polecenie domyślne ostatnio migracji. |

Poniższe przykłady aktualizują bazę danych do określonego migracji. Pierwszy używa nazwy migracji, a drugi używa Identyfikatora migracji:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>informacje kontekstu dbcontext ef DotNet

Pobiera informacje `DbContext` typu.

## <a name="dotnet-ef-dbcontext-list"></a>Lista typu dbcontext ef DotNet

Wyświetla dostępne `DbContext` typów.

## <a name="dotnet-ef-dbcontext-scaffold"></a>Tworzenie szkieletu dbcontext ef DotNet

Generuje kod dla `DbContext` i typy jednostek bazy danych. Aby to polecenie, aby wygenerować typ jednostki w tabeli bazy danych musi mieć klucz podstawowy.

Argumenty:

| Argument       | Opis                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | Parametry połączenia z bazą danych. W przypadku projektów ASP.NET Core 2.x wartość może być *nazwa =\<nazwa parametrów połączenia >* . W takim przypadku nazwa pochodzi od źródła konfiguracji, które są skonfigurowane dla projektu. |
| `<PROVIDER>`   | Dostawca do użycia. Zazwyczaj jest to nazwa pakietu NuGet, na przykład: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Opcje:

|                 | Opcja                                   | Opis                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów. Jeśli ta opcja zostanie pominięty, jest używany tylko interfejsu API fluent.                                                                |
| `-c`            | `--context <NAME>`                       | Nazwa `DbContext` klasy do wygenerowania.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | Katalog, aby umieścić `DbContext` plik klasy. Ścieżki są względne wobec katalogu projektu. Przestrzenie nazw są uzyskiwane z nazwy folderów.                                 |
| `-f`            | `--force`                                | Nadpisz istniejące pliki.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | Katalog, który można umieścić pliki klas jednostek w. Ścieżki są względne wobec katalogu projektu.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Schematów tabel w celu wygenerowania typów jednostek dla. Aby określić wiele schematów, powtórz `--schema` dla każdej z nich. Jeśli ta opcja zostanie pominięty, uwzględniono wszystkich schematów.          |
| `-t`            | `--table <TABLE_NAME>`...                | Tabele, aby wygenerować typy jednostek dla. Aby określić wiele tabel, powtórz `-t` lub `--table` dla każdej z nich. Jeśli ta opcja zostanie pominięty, wszystkie tabele są uwzględniane.                |
|                 | `--use-database-names`                   | Użyj nazwy tabel i kolumn dokładnie tak, jak pojawiają się w bazie danych. Jeśli ta opcja zostanie pominięty, nazwy baz danych zostały zmienione, aby ściślej odpowiadają Konwencji stylistycznych nazwy języka C#. |

W poniższym przykładzie scaffolds wszystkie schematy i tabele i umieszcza nowe pliki w *modeli* folderu.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Poniższy przykład szkielety mechanizmów tylko wybrane tabele i tworzy kontekst w oddzielnym folderze z określoną nazwą:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>Dodaj migracji ef DotNet

Dodaje nową migrację.

Argumenty:

| Argument | Opis                |
|:---------|:---------------------------|
| `<NAME>` | Nazwa migracji. |

Opcje:

|                   | Opcja                             | Opis                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | Katalog (i podrzędnej przestrzeni nazw) do użycia. Ścieżki są względne wobec katalogu projektu. Wartość domyślna to "Migracja". |

## <a name="dotnet-ef-migrations-list"></a>Lista migracje ef DotNet

Wyświetla listę dostępnych migracji.

## <a name="dotnet-ef-migrations-remove"></a>Usuń migracji ef DotNet

Usuwa ostatniej migracji (wycofanie zmian w kodzie, które zostały przygotowane do migracji).

Opcje:

|                   | Opcja    | Opis                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Przywróć migracji (wycofać zmiany, które zostały zastosowane do bazy danych). |

## <a name="dotnet-ef-migrations-script"></a>skrypt migracji ef DotNet

Generuje skrypt SQL z migracji.

Argumenty:

| Argument | Opis                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | Począwszy od migracji. Migracje mogą być określane według nazwy lub identyfikatora. Numer 0 jest przypadkiem szczególnym, oznacza to *przed migracją pierwszy*. Wartość domyślna to 0. |
| `<TO>`   | Końcowy migracji. Domyślnie do ostatniego migracji.                                                                                                         |

Opcje:

|                   | Opcja            | Opis                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | Plik można zapisać skryptu.                                   |
| `-i`              | `--idempotent`    | Generowanie skryptu, który może służyć w bazie danych w każdej migracji. |

Poniższy przykład tworzy skrypt migracji InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

Poniższy przykład tworzy skrypt w przypadku wszystkich migracji po migracji InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Migracje](xref:core/managing-schemas/migrations/index)
* [Odtwarzanie](xref:core/managing-schemas/scaffolding)
