---
title: Oprogramowanie .NET core interfejsu wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a>Narzędzia wiersza polecenia platformy .NET Core EF
===============================
Narzędzia wiersza polecenia programu Entity Framework Core .NET są rozszerzenia dla wielu platform **dotnet** polecenia, który jest częścią programu [.NET Core SDK][2].

> [!TIP]
> Jeśli używasz programu Visual Studio, firma Microsoft zaleca [narzędzia PMC] [ 1] zamiast ponieważ zapewniają większą integrację.

<a name="installing-the-tools"></a>Instalowanie narzędzi
--------------------
Zainstaluj narzędzia wiersza polecenia platformy .NET Core EF trzy kroki:

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

Typowe opcje:

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | --json                           | Pokaż dane wyjściowe JSON.           |
| -c | --kontekstu \<DBCONTEXT >           | DbContext do użycia.       |
| -p | --projektu \<projektu >             | Projekt do użycia.         |
| -s | --Projekt startowy \<projektu >     | Projekt startowy do użycia. |
|    | --framework \<FRAMEWORK >         | Platformy docelowej.       |
|    | --Konfiguracja \<CONFIGURATION > | Konfiguracja do użycia.   |
|    | środowisko uruchomieniowe — \<identyfikator >          | Środowiska uruchomieniowego do użycia.         |
| -h | — Pomoc                           | Pokaż informacje pomocy.      |
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
| -- | --------- | -------------------------------------------------------- |
| -f | --wymusić   | Nie potwierdzić.                                           |
|    | --przebiegu próbnego | Pokaż bazę danych, która będą pomijane, ale nie jej porzucić. |

### <a name="dotnet-ef-database-update"></a>Aktualizacja bazy danych ef DotNet

Aktualizuje bazę danych do określonego migracji.

Argumenty:

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| \<MIGRACJA > | Migracja docelowych. Jeśli 0, będzie można przywrócić wszystkich migracji. Domyślnie ostatni migracji. |

### <a name="dotnet-ef-dbcontext-info"></a>informacje o dbcontext ef DotNet

Pobiera informacje o typie DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>Lista dbcontext ef DotNet

Wyświetla listę dostępnych typów DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>szkieletu dbcontext ef DotNet

Rusztowania DbContext i jednostki typy dla bazy danych.

Argumenty:

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| \<POŁĄCZENIA > | Parametry połączenia z bazą danych.                              |
| \<DOSTAWCA >   | Dostawca do użycia. (Np. Microsoft.EntityFrameworkCore.SqlServer) |

Opcje:

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <nobr>-d</nobr> |       --adnotacji danych                | Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe). Przypadku jego pominięcia jest używana tylko interfejsu API fluent. |
|       -c        |       --kontekstu \<NAME >                 | Nazwa typu DbContext.                               |
|       -f        |       --wymusić                           | Zastąpienie istniejących plików.                                |
|       -o        |       --katalog wyjściowy \<ŚCIEŻKĘ >              | Umieścić pliki katalogu. Ścieżki są względem katalogu projektu. |
|                 | <nobr>--schematu \<SCHEMA_NAME >...</nobr> | Schematy tabele, aby wygenerować typy jednostek.      |
|       -t        |       --tabeli \<nazwa_tabeli >...          | Tabele, aby wygenerować typy jednostek.                 |
|                 |       --nazwy w przypadku baz danych użycia              | Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.   |

### <a name="dotnet-ef-migrations-add"></a>Dodaj migracje ef DotNet

Dodaje nowe migracji.

Argumenty:

|         |                            |
| ------- | -------------------------- |
| \<NAZWA > | Nazwa migracji. |

Opcje:

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <nobr>-o</nobr> | <nobr>--katalog wyjściowy \<ŚCIEŻKĘ ></nobr> | Katalog (i przestrzeni nazw sub) do użycia. Ścieżki są względem katalogu projektu. Wartość domyślna to "Migracji". |

### <a name="dotnet-ef-migrations-list"></a>Lista migracje ef DotNet

Wyświetla listę dostępnych migracji.

### <a name="dotnet-ef-migrations-remove"></a>Usuń migracje ef DotNet

Usuwa ostatniej migracji.

Opcje:

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| -f | --wymusić | Nie Sprawdź, czy migracja zostały zastosowane do bazy danych. |

### <a name="dotnet-ef-migrations-script"></a>skrypt migracje ef DotNet

Generuje skrypt SQL z migracji.

Argumenty:

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| \<Z > | Początkowy migracji. Wartość domyślna to 0 (początkowej bazy danych). |
| \<ABY >   | Końcowy migracji. Domyślnie ostatni migracji.         |

Opcje:

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| -o | --dane wyjściowe \<pliku > | Plik można zapisać wynik.                                   |
| -i | --idempotentności     | Generuj skrypt, który może być używany z bazy danych w każdej migracji. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
