---
title: .NET core interfejsu wiersza polecenia — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489612"
---
<a name="ef-core-net-command-line-tools"></a>Narzędzia wiersza polecenia programu EF Core platformy .NET
===============================
Narzędzia wiersza polecenia programu Entity Framework Core .NET to rozszerzenie dla wielu platform **dotnet** polecenia, który jest częścią programu [zestawu .NET Core SDK][2].

> [!TIP]
> Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają one bardziej zintegrowanego środowiska pracy.

<a name="installing-the-tools"></a>Instalowanie narzędzi
--------------------
> [!NOTE]
> .NET Core SDK w wersji 2.1.300 oraz nowszych **dotnet ef** poleceń, które są zgodne z programem EF Core 2.0 i nowszych wersjach. W związku z tym jeśli używane są nowe wersje programu .NET Core SDK i środowiska uruchomieniowego EF Core, instalacja nie jest wymagana, i można zignorować pozostałej części tej sekcji.
>
> Z drugiej strony **dotnet ef** narzędzie zawarte w wersji zestawu .NET Core SDK 2.1.300 i nowszych nie jest zgodny z wersją programu EF Core 1.0 i 1.1. Zanim można pracować z projektu, który używa tych starszych wersji programu EF Core na komputerze, na którym zainstalowano program .NET Core SDK 2.1.300 lub nowszej zainstalowany, należy również zainstalować wersję 2.1.200 lub starsze zestawu SDK i skonfigurować aplikację do używania tej starszej wersji, modyfikując jego  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) pliku. Ten plik zwykle znajduje się w katalogu rozwiązania (po jednym powyżej projektu). Następnie możesz przejść za pomocą instrukcji na temat poniżej.

We wcześniejszych wersjach programu .NET Core SDK można zainstalować narzędzia wiersza polecenia platformy .NET Core EF wykonując następujące kroki:

1. Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)
2. Uruchom następujące polecenia:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Projekt wynikowy powinien wyglądać mniej więcej tak:

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Odwołania do pakietu przy użyciu `PrivateAssets="All"` oznacza, że nie jest widoczne projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas programowania.

Jeśli tak, nie wszystko bezpośrednio należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Przy użyciu narzędzi
---------------
Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty:

Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte). Projekt docelowy domyślnie do projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **`--project`** </nobr> opcji.

Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu. On również wartość domyślna to projekt w bieżącym katalogu, ale można zmienić za pomocą **`--startup-project`** opcji.

> [!NOTE]
> Na przykład aktualizowanie bazy danych zawierającej programu EF Core zainstalowany w innym projekcie aplikacji sieci web będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)

Typowe opcje:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | Pokaż dane wyjściowe JSON.           |
| -c | `--context <DBCONTEXT>`           | Kontekst DbContext do użycia.       |
| -p | `--project <PROJECT>`             | Projekt do użycia.         |
| -s | `--startup-project <PROJECT>`     | Projekt startowy do użycia. |
|    | `--framework <FRAMEWORK>`         | Platforma docelowa.       |
|    | `--configuration <CONFIGURATION>` | Konfiguracja do użycia.   |
|    | `--runtime <IDENTIFIER>`          | Środowisko uruchomieniowe do użycia.         |
| -h | `--help`                           | Pokaż informacje pomocy.      |
| -v | `--verbose`                        | Wyświetlić pełne dane wyjściowe.        |
|    | `--no-color`                       | Nie kolorować danych wyjściowych.      |
|    | `--prefix-output`                  | Prefiks danych wyjściowych z poziomu.   |


> [!TIP]
> Aby określić środowiska ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.

<a name="commands"></a>Polecenia
--------

### <a name="dotnet-ef-database-drop"></a>docelowej bazie danych ef DotNet

Porzuca bazy danych.

Opcje:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | `--force`   | Nie Potwierdź.                                           |
|    | `--dry-run` | Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej. |

### <a name="dotnet-ef-database-update"></a>Aktualizacja bazy danych programu ef DotNet

Aktualizuje bazę danych do określonego migracji.

Argumenty:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Migracja docelowego. Jeśli jest to 0, będzie można przywrócić wszystkich migracji. Domyślnie do ostatniego migracji. |

### <a name="dotnet-ef-dbcontext-info"></a>informacje kontekstu dbcontext ef DotNet

Pobiera informacje o typie DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>Lista typu dbcontext ef DotNet

Wyświetla listę dostępnych typów DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>Tworzenie szkieletu dbcontext ef DotNet

Szkielety mechanizmów DbContext i jednostek typów dla bazy danych.

Argumenty:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | Parametry połączenia z bazą danych.                                      |
| `<PROVIDER>`   | Dostawca do użycia. (na przykład Microsoft.EntityFrameworkCore.SqlServer) |

Opcje:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                      | Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów. W przypadku pominięcia jest używana tylko interfejsu API fluent. |
| -c              | `--context <NAME>`                       | Nazwa typu DbContext.                                                                       |
|                 | `--context-dir <PATH>`                   | Katalog, który można umieścić plik typu DbContext w. Ścieżki są względne wobec katalogu projektu.             |
| -f              | `--force`                                 | Nadpisz istniejące pliki.                                                                        |
| -o              | `--output-dir <PATH>`                    | Katalog, który można umieścić pliki w. Ścieżki są względne wobec katalogu projektu.                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Schematów tabel w celu wygenerowania typów jednostek dla.                                              |
| -t              | `--table <TABLE_NAME>`...                | Tabele, aby wygenerować typy jednostek dla.                                                         |
|                 | `--use-database-names`                    | Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.                                           |

### <a name="dotnet-ef-migrations-add"></a>Dodaj migracji ef DotNet

Dodaje nową migrację.

Argumenty:

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | Nazwa migracji. |

Opcje:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr> `--output-dir <PATH>` </nobr> | Katalog (i podrzędnej przestrzeni nazw) do użycia. Ścieżki są względne wobec katalogu projektu. Wartość domyślna to "Migracja". |

### <a name="dotnet-ef-migrations-list"></a>Lista migracje ef DotNet

Wyświetla listę dostępnych migracji.

### <a name="dotnet-ef-migrations-remove"></a>Usuń migracji ef DotNet

Usuwa ostatni migracji.

Opcje:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | `--force` | Przywróć migracji, jeśli został zastosowany do bazy danych. |

### <a name="dotnet-ef-migrations-script"></a>skrypt migracji ef DotNet

Generuje skrypt SQL z migracji.

Argumenty:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | Począwszy od migracji. Wartość domyślna to 0 (początkowej bazy danych). |
| `<TO>`   | Końcowy migracji. Domyślnie do ostatniego migracji.         |

Opcje:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | `--output <FILE>` | Plik można zapisać wynik.                                   |
| -i | `--idempotent`     | Generowanie skryptu, który może służyć w bazie danych w każdej migracji. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
