---
title: "Konsola Menedżera pakietów (Visual Studio) — podstawowe EF"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a>Narzędzia konsoli Menedżera pakietów Core EF
=====================================
Uruchom narzędzia konsoli Menedżera pakietów EF Core (PMC) w programie Visual Studio przy użyciu narzędzia NuGet [Konsola Menedżera pakietów][2].
Te narzędzia Praca z projektami zarówno .NET Framework i .NET Core.

> [!TIP]
> Nie używasz programu Visual Studio? [EF podstawowe narzędzia wiersza polecenia] [ 1] są międzyplatformowa i uruchamiane w wierszu polecenia.

<a name="installing-the-tools"></a>Instalowanie narzędzi
--------------------
Zainstaluj narzędzia konsoli Menedżera pakietów Core EF, instalując pakiet Microsoft.EntityFrameworkCore.Tools NuGet.
Można ją zainstalować, wykonując następujące polecenie w [Konsola Menedżera pakietów][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Jeśli wszystko działało poprawnie, należy uruchomić to polecenie:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Jeśli Twój projekt startowy jest przeznaczony dla platformy .NET Standard [między celem obsługiwanych struktur] [ 3] przed użyciem narzędzia.

> [!IMPORTANT]
> Jeśli używasz **uniwersalnych systemu Windows** lub **Xamarin**, Przenieś swój kod EF do .NET Standard biblioteki klas i [między celem obsługiwanych struktur] [ 3] przed użyciem narzędzia. Określ biblioteki klas jako projekt startowy.

<a name="using-the-tools"></a>Korzystając z narzędzi
---------------
Przy każdym wywołaniu polecenia obejmuje dwa projekty:

Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte). Domyślnie projektu docelowego **domyślny projekt** wybrany w konsoli Menedżera pakietów, ale można również określić za pomocą parametru - projektu.

Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu. Domyślnie jedną **Ustaw jako projekt startowy** w Eksploratorze rozwiązań. Można również określić, za pomocą parametru - StartupProject.

Wspólne parametry:

|                           |                             |
| ------------------------- | --------------------------- |
| -Kontekst \<ciąg >        | DbContext do użycia.       |
| -Projektu \<ciąg >        | Projekt do użycia.         |
| -StartupProject \<ciąg > | Projekt startowy do użycia. |
| -Verbose                  | Pokaż pełne dane wyjściowe.        |

Aby wyświetlić Pomoc informacje dotyczące polecenia, za pomocą programu PowerShell w `Get-Help` polecenia.

> [!TIP]
> Parametry kontekstu, projektu i StartupProject obsługuje kartę rozszerzenia.

> [!TIP]
> Ustaw **env:ASPNETCORE_ENVIRONMENT** przed uruchomieniem do określenia środowiska ASP.NET Core.

<a name="commands"></a>Polecenia
--------

### <a name="add-migration"></a>Dodaj migracji

Dodaje nowe migracji.

Parametry:

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| ***-Nazwa*** \<ciąg >              | Nazwa migracji.                                                      |
| <nobr>-OutputDir \<ciąg ></nobr>  | Katalog (i przestrzeni nazw sub) do użycia. Ścieżki są względem katalogu projektu. Wartość domyślna to "Migracji". |

> [!NOTE]
> Parametry w **bold** są wymagane i w *italics* są pozycyjnych.

### <a name="drop-database"></a>Baza danych listy

Odrzuca bazy danych.

Parametry:

|          |                                                          |
| -------- | -------------------------------------------------------- |
| -WhatIf  | Pokaż bazę danych, która będą pomijane, ale nie jej porzucić. |

### <a name="get-dbcontext"></a>Get-DbContext

Pobiera informacje o typie DbContext.

### <a name="remove-migration"></a>Usuń migracji

Usuwa ostatniej migracji.

Parametry:

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| -Force. | Nie Sprawdź, czy migracja zostały zastosowane do bazy danych. |

### <a name="scaffold-dbcontext"></a>Szkieletu DbContext

Rusztowania DbContext i jednostki typy dla bazy danych.

Parametry:

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <nobr>***-Połączenia*** \<ciąg ></nobr> | Parametry połączenia z bazą danych.                                    |
| ***-Dostawca*** \<ciąg >                | Dostawca do użycia. (Np. Microsoft.EntityFrameworkCore.SqlServer)       |
| -OutputDir \<ciąg >                     | Umieścić pliki katalogu. Ścieżki są względem katalogu projektu. |
| -Kontekst \<ciąg >                       | Nazwa typu DbContext w celu wygenerowania.                                    |
| -Schematy \<String [] >                     | Schematy tabele, aby wygenerować typy jednostek.                       |
| -Tabele \<String [] >                      | Tabele, aby wygenerować typy jednostek.                                  |
| -DataAnnotations                         | Użyj atrybutów, aby skonfigurować model (jeśli będzie to możliwe). Przypadku jego pominięcia jest używana tylko interfejsu API fluent. |
| -UseDatabaseNames                        | Użyj nazwy tabel i kolumn bezpośrednio z bazy danych.                    |
| -Force.                                   | Zastąpienie istniejących plików.                                                 |

### <a name="script-migration"></a>Skrypt migracji

Generuje skrypt SQL z migracji.

Parametry:

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| *-From* \<ciąg > | Początkowy migracji. Wartość domyślna to 0 (początkowej bazy danych).      |
| *— Do* \<ciąg >   | Końcowy migracji. Domyślnie ostatni migracji.              |
| -Idempotentności       | Generuj skrypt, który może być używany z bazy danych w każdej migracji. |
| -Output \<ciąg > | Plik można zapisać wynik.                                   |

> [!TIP]
> Do z, i parametry wyjściowe obsługuje kartę rozszerzenia.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <nobr>*-Migration* \<ciąg ></nobr> | Migracja docelowych. W przypadku wartości "0" wszystkich migracji zostaną cofnięte. Domyślnie ostatni migracji. |

> [!TIP]
> Parametr migracji obsługuje kartę rozszerzenia.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
