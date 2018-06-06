---
title: Oprogramowanie .NET core interfejsu wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754499"
---
<a name="ef-core-net-command-line-tools"></a>Narzędzia wiersza polecenia platformy .NET Core EF
===============================
Narzędzia wiersza polecenia programu Entity Framework Core .NET są rozszerzenia dla wielu platform **dotnet** polecenia, który jest częścią programu [.NET Core SDK][2].

> [!TIP]
> Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają większą integrację.

<a name="installing-the-tools"></a>Instalowanie narzędzi
--------------------
> [!NOTE]
> .NET Core SDK w wersji 2.1.300 i zawiera nowszą **dotnet ef** poleceń, które są zgodne z EF Core w wersji 2.0 i nowszych wersjach. Dlatego jeśli używane są nowe wersje zestawu SDK .NET Core i EF podstawowego środowiska wykonawczego, instalacja nie jest wymagana i pozostałej części tej sekcji można zignorować.
>
> Z drugiej strony **dotnet ef** narzędzie zawartych w wersji zestawu SDK programu .NET Core 2.1.300 i nowszych nie jest zgodny z wersją EF Core 1.0 i 1.1. Przed możesz pracować z projektu, który używa tych wersji EF rdzeni na komputerze, na którym zainstalowano program .NET Core SDK 2.1.300 lub nowszej zainstalowany, należy również zainstalować wersję 2.1.200 lub starsze zestawu SDK i skonfigurować aplikację do używania starszej wersji, modyfikując jego  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) pliku. Ten plik znajduje się zwykle w katalogu rozwiązania (jeden nad projektu). Następnie można przejść z poniższych instrukcji na temat.

W poprzednich wersjach zestawu SDK .NET Core można zainstalować narzędzi wiersza polecenia platformy .NET Core EF przy użyciu następujące kroki:

1. Edytuj plik projektu i dodać Microsoft.EntityFrameworkCore.Tools.DotNet jako element DotNetCliToolReference (patrz poniżej)
2. Uruchom następujące polecenia:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Projekt wynikowy powinien wyglądać następująco:

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
> Odwołanie pakietu z `PrivateAssets="All"` oznacza, że nie jest on ujawniony dla projektów odwołujących się do tego projektu, co jest szczególnie przydatne w przypadku pakietów, które zwykle są używane tylko podczas tworzenia.

Jeśli tak, nie wszystkie prawa należy pomyślnie uruchomić następujące polecenie w wierszu polecenia.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Korzystając z narzędzi
---------------
Przy każdym wywołaniu polecenia obejmuje dwa projekty:

Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte). Docelowy projekt domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą <nobr> **--projektu** </nobr> opcji.

Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu. On również domyślnie projektu w bieżącym katalogu, ale można zmienić za pomocą **— projekt startowy** opcji.

> [!NOTE]
> Na przykład uaktualnienie bazy danych aplikacji sieci web, który został zainstalowany w innym projekcie Core EF będzie wyglądać następująco: `dotnet ef database update --project {project-path}` (z katalogu aplikacji sieci web)

Typowe opcje:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Pokaż dane wyjściowe JSON.           |
| -c | --kontekstu \<DBCONTEXT >           | DbContext do użycia.       |
| -p | --projektu \<projektu >             | Projekt do użycia.         |
| -s | --Projekt startowy \<projektu >     | Projekt startowy do użycia. |
|    | --framework \<FRAMEWORK >         | Platforma docelowa.       |
|    | --Konfiguracja \<CONFIGURATION > | Konfiguracja do użycia.   |
|    | środowisko uruchomieniowe — \<identyfikator >          | Środowiska uruchomieniowego do użycia.         |
| -h | --help                           | Pokaż informacje pomocy.      |
| -v | -verbose                        | Pokaż pełne dane wyjściowe.        |
|    | — Brak koloru                       | Nie kolorowanie danych wyjściowych.      |
|    | --dane wyjściowe prefiksu                  | Dane wyjściowe z poziomu prefiks.   |


> [!TIP]
> Aby w środowisku ASP.NET Core, ustaw **ASPNETCORE_ENVIRONMENT** zmiennej środowiskowej przed uruchomieniem.

<a name="commands"></a>Polecenia
--------

### <a name="dotnet-ef-database-drop"></a>Usuń bazę danych ef DotNet

Odrzuca bazy danych.

Opcje:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --wymusić   | Nie potwierdzić.                                           |
|    | --przebiegu próbnego | Pokaż bazę danych, która będą pomijane, ale nie jej porzucić. |

### <a name="dotnet-ef-database-update"></a>Aktualizacja bazy danych ef DotNet

Aktualizuje bazę danych do określonego migracji.

Argumenty:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRACJA &GT; | Migracja docelowych. Jeśli 0, będzie można przywrócić wszystkich migracji. Domyślnie ostatni migracji. |

### <a name="dotnet-ef-dbcontext-info"></a>informacje o dbcontext ef DotNet

Pobiera informacje o typie DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>Lista dbcontext ef DotNet

Wyświetla listę dostępnych typów DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>szkieletu dbcontext ef DotNet

Rusztowania DbContext i jednostki typy dla bazy danych.

Argumenty:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<POŁĄCZENIA &GT; | Parametry połączenia z bazą danych.                                      |
| \<DOSTAWCA &GT;   | Dostawca do użycia. (na przykład Microsoft.EntityFrameworkCore.SqlServer) |

Opcje:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --adnotacji danych                      | Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe). Przypadku jego pominięcia jest używana tylko interfejsu API fluent. |
| -c              | --kontekstu \<NAME >                       | Nazwa typu DbContext.                                                                       |
|                 | -dir kontekstu \<ŚCIEŻKĘ >                   | Katalog mają zostać umieszczone w pliku DbContext. Ścieżki są względem katalogu projektu.             |
| -f              | --wymusić                                 | Zastąpienie istniejących plików.                                                                        |
| -o              | --katalog wyjściowy \<ŚCIEŻKĘ >                    | Umieścić pliki katalogu. Ścieżki są względem katalogu projektu.                      |
|                 | <nobr>--schematu \<SCHEMA_NAME >...</nobr> | Schematy tabele, aby wygenerować typy jednostek.                                              |
| -t              | --tabeli \<nazwa_tabeli >...                | Tabele, aby wygenerować typy jednostek.                                                         |
|                 | --nazwy w przypadku baz danych użycia                    | Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.                                           |

### <a name="dotnet-ef-migrations-add"></a>Dodaj migracje ef DotNet

Dodaje nowe migracji.

Argumenty:

|         |                            |
|:--------|:---------------------------|
| \<NAME> | Nazwa migracji. |

Opcje:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--katalog wyjściowy \<ŚCIEŻKĘ ></nobr> | Katalog (i przestrzeni nazw sub) do użycia. Ścieżki są względem katalogu projektu. Wartość domyślna to "Migracji". |

### <a name="dotnet-ef-migrations-list"></a>Lista migracje ef DotNet

Wyświetla listę dostępnych migracji.

### <a name="dotnet-ef-migrations-remove"></a>Usuń migracje ef DotNet

Usuwa ostatniej migracji.

Opcje:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --wymusić | Przywróć migracji, jeśli została zastosowana do bazy danych. |

### <a name="dotnet-ef-migrations-script"></a>skrypt migracje ef DotNet

Generuje skrypt SQL z migracji.

Argumenty:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<Z &GT; | Początkowy migracji. Wartość domyślna to 0 (początkowej bazy danych). |
| \<ABY &GT;   | Końcowy migracji. Domyślnie ostatni migracji.         |

Opcje:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --dane wyjściowe \<pliku > | Plik można zapisać wynik.                                   |
| -i | --idempotentności     | Generuj skrypt, który może być używany z bazy danych w każdej migracji. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
