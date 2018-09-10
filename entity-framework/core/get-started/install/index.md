---
title: Instalowanie Entiy Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250325"
---
# <a name="installing-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

## <a name="prerequisites"></a>Wymagania wstępne

* Do tworzenia aplikacji przeznaczonych dla platformy .NET Core 2.1, należy zainstalować [zestawu SDK programu .NET Core 2.1](https://www.microsoft.com/net/download/core). Zestaw SDK został zainstalowany, nawet jeśli masz najnowszą wersję programu Visual Studio 2017.

* Aby użyć programu Visual Studio do tworzenia aplikacji przeznaczonych dla platformy .NET Core 2.1, należy zainstalować Visual Studio 2017 w wersji 15.7 lub nowszej.

* Aby używać programu Entity Framework 2.1 w aplikacji platformy ASP.NET Core, należy użyć platformy ASP.NET Core 2.1. Aplikacje, które używają starszych wersji platformy ASP.NET Core, musi zostać zaktualizowany do wersji 2.1.

* Można użyć programu Visual Studio 2015 dla aplikacji przeznaczonych dla platformy .NET Framework 4.6.1 lub nowszej. Jednak potrzebna jest wersja pakietu nuget, rozpoznający .NET Standard 2.0 i jego struktury zgodne. Aby uzyskać tę w programie Visual Studio 2015 [uaktualnić klienta programu NuGet w wersji 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Pobierz środowisko uruchomieniowe programu Entity Framework Core

Aby dodać biblioteki środowiska uruchomieniowego EF Core do aplikacji, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć. Aby uzyskać listę obsługiwanych dostawców i ich nazwy pakietów NuGet, zobacz [bazy danych dostawcy](../../providers/index.md).

Instalowanie lub aktualizowanie pakietów NuGet, przy użyciu interfejsu wiersza polecenia platformy .NET Core, okno dialogowe programu Visual Studio pakietu Menedżera lub konsoli Menedżera pakietów Visual Studio.

W przypadku aplikacji platformy ASP.NET Core 2.1 w pamięci i dostawcy programu SQL Server są automatycznie dołączane, więc nie trzeba instalować ich osobno.

> [!TIP]  
> Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych innej firmy, Zawsze sprawdzaj dostępność aktualizacji dostawcy, który jest zgodny z wersją programu EF Core, którego chcesz użyć. Na przykład dostawcy baz danych dla wcześniejszych wersji nie są zgodne z środowiska uruchomieniowego EF Core w wersji 2.1.  

### <a name="net-core-cli"></a>.NET core interfejsu wiersza polecenia

Następujące polecenie interfejsu wiersza polecenia platformy .NET Core instaluje lub aktualizuje dostawcy programu SQL Server:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Można wskazać określonej wersji w `dotnet add package` polecenia przy użyciu `-v` modyfikator. Na przykład, aby zainstalować pakiety programu EF Core 2.1.0, należy dołączyć `-v 2.1.0` do polecenia.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Okno Menedżera pakietów NuGet programu Visual Studio

* W menu, wybierz opcję **Projekt > Zarządzaj pakietami NuGet**

* Kliknij pozycję **Przeglądaj** lub **aktualizacje** kartę

* Aby zainstalować lub zaktualizować dostawcy programu SQL Server, wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i potwierdź.

Aby uzyskać więcej informacji, zobacz [okno Menedżera pakietów NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konsola Menedżera pakietów NuGet dla programu Visual Studio

* W menu, wybierz opcję **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Aby zainstalować dostawcę programu SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Aby zaktualizować dostawcę, użyj `Update-Package` polecenia.

* Aby określić określonej wersji, użyj `-Version` modyfikator. Na przykład, aby zainstalować pakiety programu EF Core 2.1.0, należy dołączyć `-Version 2.1.0` poleceń

Aby uzyskać więcej informacji, zobacz [Konsola Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Pobierz narzędzia platformy Entity Framework Core

Oprócz bibliotek środowiska uruchomieniowego można zainstalować narzędzia, które mogą wykonywać niektóre zadania związane z programem EF Core w projekcie w czasie projektowania. Na przykład można utworzyć migracje, zastosować migracje i utworzyć model, w oparciu o istniejącą bazę danych.

Dostępne są dwa zestawy narzędzi:
* .NET Core [narzędzi interfejsu wiersza polecenia (CLI)](../../miscellaneous/cli/dotnet.md) może służyć w Windows, Linux lub macOS. Te polecenia zaczynają się od `dotnet ef`. 
* [Narzędzia Konsola Menedżera pakietów](../../miscellaneous/cli/powershell.md) systemem Windows w programie Visual Studio 2017. Te polecenia rozpoczynać czasownika, na przykład `Add-Migration`, `Update-Database`.

Chociaż można używać `dotnet ef` poleceń z poziomu konsoli Menedżera pakietów jest bardziej wygodne do korzystania z narzędzi Konsola Menedżera pakietów, podczas korzystania z programu Visual Studio:
* Działają automatycznie z bieżącego projektu wybrany w konsoli Menedżera pakietów, bez konieczności ręcznie zmienić katalog.  
* One automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Pobierz narzędzia interfejsu wiersza polecenia

`dotnet ef` Poleceń są zawarte w zestawie SDK programu .NET Core, ale aby włączyć polecenia należy zainstalować `Microsoft.EntityFrameworkCore.Design` pakietu:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

W przypadku aplikacji platformy ASP.NET Core 2.1 Ten pakiet jest uwzględniane automatycznie.

Jak wyjaśniono wcześniej w [wymagania wstępne](#prerequisites), należy także zainstalować zestaw SDK programu .NET Core 2.1.

> [!IMPORTANT]      
> Zawsze używaj wersji zgodnej wersji głównej pakiety środowiska uruchomieniowego pakietu Narzędzia.

### <a name="get-the-package-manager-console-tools"></a>Pobierz narzędzia w konsoli Menedżera pakietów

Aby uzyskać Konsola Menedżera pakietów narzędzi dla platformy EF Core, należy zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

W przypadku aplikacji platformy ASP.NET Core 2.1 Ten pakiet jest uwzględniane automatycznie.

## <a name="upgrading-to-ef-core-21"></a>Uaktualnianie do EF Core 2.1

Jeśli aktualizujesz istniejącą aplikację platformy EF Core 2.1 niektórych odwołań do starszych pakietów programu EF Core może być konieczne ręczne usunięcie:

* Bazy danych dostawcy projektowania pakietów, takich jak `Microsoft.EntityFrameworkCore.SqlServer.Design` nie jest już wymagane lub obsługiwanych w programie EF Core 2.1, ale nie są automatycznie usuwane po uaktualnieniu innych pakietów.

* Narzędzia wiersza polecenia platformy .NET zawiera teraz zestaw .NET SDK, odwołanie do tego pakietu może zostać usunięty z *.csproj* pliku:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

W przypadku aplikacji dla środowiska .NET Framework, które zostały utworzone we wcześniejszych wersjach programu Visual Studio upewnij się, że są one zgodne z bibliotekami .NET Standard 2.0:

  * Edytuj plik projektu i upewnij się, że w grupie właściwości początkowe pojawia się następujący wpis:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Dla projektów testowych również upewnij się, że występuje następujący wpis:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
