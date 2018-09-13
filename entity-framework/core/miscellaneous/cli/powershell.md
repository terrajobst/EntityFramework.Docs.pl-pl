---
title: Konsola Menedżera pakietów (Visual Studio) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490856"
---
<a name="ef-core-package-manager-console-tools"></a>Narzędzia konsoli Menedżera pakietów programu EF Core
=====================================
Uruchomione narzędzia konsoli Menedżera pakietów (PMC) EF Core w programie Visual Studio za pomocą NuGet [Konsola Menedżera pakietów][2].
Te narzędzia działają w projektach .NET Core i .NET Framework.

> [!TIP]
> Nie można za pomocą programu Visual Studio? [Narzędzi wiersza polecenia programu EF Core] [ 1] dla wielu platform i wykonywania w wierszu polecenia.

<a name="installing-the-tools"></a>Instalowanie narzędzi
--------------------
Zainstaluj narzędzia konsoli Menedżera pakietów programu EF Core, instalując pakiet Microsoft.EntityFrameworkCore.Tools NuGet.
Można ją zainstalować, wykonując następujące polecenie z poziomu [Konsola Menedżera pakietów][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Jeśli wszystko działało poprawnie, należy uruchomić to polecenie:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Jeśli Twój projekt startowy jest przeznaczony dla .NET Standard [cross-target framework obsługiwane] [ 3] przed rozpoczęciem korzystania z narzędzia.

> [!IMPORTANT]
> Jeśli używasz **Universal Windows** lub **Xamarin**, Przenieś swój kod programem EF do biblioteki klas .NET Standard i [cross-target framework obsługiwane] [ 3] przed rozpoczęciem korzystania z narzędzia. Określ bibliotekę klas jako projekt startowy.

<a name="using-the-tools"></a>Przy użyciu narzędzi
---------------
Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty:

Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte). Wartością domyślną jest projekt docelowy **domyślny projekt** wybrany w konsoli Menedżera pakietów, ale można również określić za pomocą parametru - projekt.

Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu. Jego wartość domyślna to jeden **Ustaw jako projekt startowy** w Eksploratorze rozwiązań. Można również określić, za pomocą parametru - projekt startowy.

Wspólne parametry:

|                           |                             |
|:--------------------------|:----------------------------|
| -Kontekstu \<ciąg >        | Kontekst DbContext do użycia.       |
| -Projekt \<ciąg >        | Projekt do użycia.         |
| Projekt startowy - \<ciąg > | Projekt startowy do użycia. |
| -Verbose                  | Wyświetlić pełne dane wyjściowe.        |

Aby wyświetlić Pomoc dotyczącą polecenia, użyj programu PowerShell `Get-Help` polecenia.

> [!TIP]
> Parametry kontekstu, projekt i projekt startowy obsługują rozszerzenia karty.

> [!TIP]
> Ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem do określania środowiska ASP.NET Core.

<a name="commands"></a>Polecenia
--------

### <a name="add-migration"></a>Dodaj migracji

Dodaje nową migrację.

Parametry:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***— Nazwa*** \<ciąg >             | Nazwa migracji.                                                                                       |
| <nobr>-OutputDir \<ciąg ></nobr> | Katalog (i podrzędnej przestrzeni nazw) do użycia. Ścieżki są względne wobec katalogu projektu. Wartość domyślna to "Migracja". |

> [!NOTE]
> Parametry w **bold** są wymagane i te w *Kursywa* są pozycyjnych.

### <a name="drop-database"></a>Baza danych listy

Porzuca bazy danych.

Parametry:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Pokaż bazę danych, która będzie można usunąć, ale nie należy usuwać jej. |

### <a name="get-dbcontext"></a>Get-DbContext

Pobiera informacje o typie DbContext.

### <a name="remove-migration"></a>Usuń migrację

Usuwa ostatni migracji.

Parametry:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Przywróć migracji, jeśli został zastosowany do bazy danych. |

### <a name="scaffold-dbcontext"></a>Tworzenie szkieletu DbContext

Szkielety mechanizmów DbContext i jednostek typów dla bazy danych.

Parametry:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Connection*** \<ciąg ></nobr> | Parametry połączenia z bazą danych.                                                           |
| ***-Provider*** \<ciąg >                | Dostawca do użycia. (na przykład Microsoft.EntityFrameworkCore.SqlServer)                      |
| -OutputDir \<ciąg >                     | Katalog, który można umieścić pliki w. Ścieżki są względne wobec katalogu projektu.                      |
| -ContextDir \<ciąg >                    | Katalog, który można umieścić plik typu DbContext w. Ścieżki są względne wobec katalogu projektu.             |
| -Kontekstu \<ciąg >                       | Nazwa typu DbContext do wygenerowania.                                                           |
| -Schematów \<String [] >                     | Schematów tabel w celu wygenerowania typów jednostek dla.                                              |
| -Tabele \<String [] >                      | Tabele, aby wygenerować typy jednostek dla.                                                         |
| -DataAnnotations                         | Aby skonfigurować model (tam, gdzie jest to możliwe), należy użyć atrybutów. W przypadku pominięcia jest używana tylko interfejsu API fluent. |
| -UseDatabaseNames                        | Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.                                           |
| -Force                                   | Nadpisz istniejące pliki.                                                                        |

### <a name="script-migration"></a>Skrypt migracji

Generuje skrypt SQL z migracji.

Parametry:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<ciąg > | Począwszy od migracji. Wartość domyślna to 0 (początkowej bazy danych).      |
| *— Do* \<ciąg >   | Końcowy migracji. Domyślnie do ostatniego migracji.              |
| -Idempotentne       | Generowanie skryptu, który może służyć w bazie danych w każdej migracji. |
| -Dane wyjściowe \<ciąg > | Plik można zapisać wynik.                                   |

> [!TIP]
> To, z danych wyjściowych z obsługą i parametrów rozszerzenia karty.

### <a name="update-database"></a>Aktualizuj bazy danych

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*— Migracja* \<ciąg ></nobr> | Migracja docelowego. Jeśli jest to "0", będzie można przywrócić wszystkich migracji. Domyślnie do ostatniego migracji. |

> [!TIP]
> Parametr migracji obsługuje rozszerzenia karty.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
