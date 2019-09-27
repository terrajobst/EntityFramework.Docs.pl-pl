---
title: Instalowanie Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7bdedf563b7d919ba334db79af73c3eed3ba4129
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317580"
---
# <a name="installing-entity-framework-core"></a>Instalowanie Entity Framework Core

## <a name="prerequisites"></a>Wymagania wstępne

* EF Core jest biblioteką [.NET Standard 2,1](/dotnet/standard/net-standard) . Dlatego EF Core wymaga implementacji platformy .NET obsługującej .NET Standard 2,1 do uruchomienia. EF Core można także odwoływać się do innych bibliotek .NET Standard 2,1. 

* Na przykład można użyć EF Core do tworzenia aplikacji przeznaczonych dla platformy .NET Core. Tworzenie aplikacji .NET Core wymaga [zestaw .NET Core SDK](https://dotnet.microsoft.com/download). Opcjonalnie możesz również użyć środowiska programistycznego, takiego jak [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac)lub [Visual Studio Code](https://code.visualstudio.com). Aby uzyskać więcej informacji, sprawdź [wprowadzenie z platformą .NET Core](/dotnet/core/get-started).

* Za pomocą programu Visual Studio EF Core można opracowywać aplikacje w systemie Windows. Zalecana jest Najnowsza wersja programu [Visual Studio](https://visualstudio.microsoft.com/vs) .

* EF Core można uruchamiać na innych implementacjach platformy .NET, takich jak [Xamarin](https://dotnet.microsoft.com/apps/xamarin) i .NET Native. Jednak w ramach tych implementacji obowiązują ograniczenia środowiska uruchomieniowego, które mogą mieć wpływ na działanie EF Core w aplikacji. Aby uzyskać więcej informacji, zobacz [implementacje platformy .NET obsługiwane przez EF Core](xref:core/platforms/index).

* Na koniec różni dostawcy baz danych mogą wymagać określonych wersji aparatu bazy danych, implementacji platformy .NET lub systemów operacyjnych. Upewnij się, że [dostawca bazy danych EF Core](xref:core/providers/index) jest dostępny, który obsługuje odpowiednie środowisko dla aplikacji.

## <a name="get-the-entity-framework-core-runtime"></a>Pobierz środowisko uruchomieniowe Entity Framework Core

Aby dodać EF Core do aplikacji, zainstaluj pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.

W przypadku kompilowania aplikacji ASP.NET Core nie trzeba instalować dostawców znajdujących się w pamięci i SQL Server. Te dostawcy są dołączane do bieżących wersji ASP.NET Core wraz z EF Core środowiska uruchomieniowego.  

Aby zainstalować lub zaktualizować pakiety NuGet, można użyć interfejsu wiersza polecenia platformy .NET Core (CLI), okna dialogowego Menedżera pakietów programu Visual Studio lub konsoli Menedżera pakietów programu Visual Studio.

### <a name="net-core-cli"></a>Interfejs wiersza polecenia platformy .NET Core

* Użyj następującego polecenia interfejs wiersza polecenia platformy .NET Core w wierszu polecenia systemu operacyjnego, aby zainstalować lub zaktualizować dostawcę EF Core SQL Server:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Możesz wskazać określoną wersję w `dotnet add package` poleceniu, `-v` używając modyfikatora. Na przykład aby zainstalować EF Core pakiety 2.2.0, Dołącz `-v 2.2.0` do polecenia.

Aby uzyskać więcej informacji, zobacz [Narzędzia interfejsu wiersza polecenia (CLI) platformy .NET](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Okno dialogowe Menedżera pakietów NuGet programu Visual Studio

* Z menu programu Visual Studio wybierz pozycję **projekt > Zarządzaj pakietami NuGet** .

* Kliknij kartę **Przeglądaj** lub **aktualizacje**

* Aby zainstalować lub zaktualizować dostawcę SQL Server, wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakiet i potwierdź.

Aby uzyskać więcej informacji, zobacz [okno dialogowe Menedżera pakietów NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konsola Menedżera pakietów NuGet programu Visual Studio

* Z menu programu Visual Studio wybierz kolejno pozycje **narzędzia > Menedżer pakietów NuGet > konsola Menedżera pakietów**

* Aby zainstalować dostawcę SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Aby zaktualizować dostawcę, użyj `Update-Package` polecenia.

* Aby określić określoną wersję, użyj `-Version` modyfikatora. Na przykład aby zainstalować EF Core pakiety 2.2.0, Dołącz `-Version 2.2.0` do poleceń

Aby uzyskać więcej informacji, zobacz [konsola Menedżera pakietów](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Pobierz narzędzia Entity Framework Core

Możesz zainstalować narzędzia do wykonywania zadań związanych z EF Core w projekcie, takich jak tworzenie i stosowanie migracji baz danych lub tworzenie modelu EF Core na podstawie istniejącej bazy danych.

Dostępne są dwa zestawy narzędzi:

* [Narzędzia interfejsu wiersza polecenia platformy .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) mogą być używane w systemie Windows, Linux lub macOS. Te polecenia zaczynają `dotnet ef`się od. 

* [Narzędzia konsoli Menedżera pakietów (PMC)](xref:core/miscellaneous/cli/powershell) działają w programie Visual Studio w systemie Windows. Te polecenia zaczynają się od zlecenia, na `Add-Migration`przykład `Update-Database`.

Mimo że można także użyć `dotnet ef` poleceń z konsoli Menedżera pakietów, zaleca się korzystanie z narzędzi konsoli Menedżera pakietów w przypadku korzystania z programu Visual Studio:

* Automatycznie pracują z bieżącym projektem wybranym w ramach dyrektywy PMC w programie Visual Studio, bez konieczności ręcznego przełączania katalogów.  

* Po zakończeniu tego polecenia automatycznie otwierane są pliki wygenerowane przez polecenia w programie Visual Studio.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Pobierz narzędzia interfejs wiersza polecenia platformy .NET Core

Narzędzia interfejs wiersza polecenia platformy .NET Core wymagają zestaw .NET Core SDK, wymienione wcześniej w [wymaganiach wstępnych](#prerequisites).

Polecenia są zawarte w bieżących wersjach zestaw .NET Core SDK, ale aby włączyć polecenia w określonym projekcie, należy `Microsoft.EntityFrameworkCore.Design` zainstalować pakiet: `dotnet ef`

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

W przypadku aplikacji ASP.NET Core ten pakiet jest uwzględniany automatycznie.

> [!IMPORTANT]      
> Zawsze używaj wersji pakietu narzędzi, która jest zgodna z wersją główną pakietów środowiska uruchomieniowego.

### <a name="get-the-package-manager-console-tools"></a>Pobierz narzędzia konsoli Menedżera pakietów

Aby uzyskać narzędzia konsoli Menedżera pakietów dla EF Core, zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakiet. Na przykład w programie Visual Studio:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

W przypadku aplikacji ASP.NET Core ten pakiet jest uwzględniany automatycznie.

## <a name="upgrading-to-the-latest-ef-core"></a>Uaktualnianie do najnowszej EF Core

* Za każdym razem, gdy udostępnimy nową wersję EF Core, wybraliśmy również nową wersję dostawców, którzy są częścią projektu EF Core, takich jak Microsoft. EntityFrameworkCore. SqlServer, Microsoft. EntityFrameworkCore. SQLite, i Microsoft. EntityFrameworkCore. inMemory. Po prostu możesz przeprowadzić uaktualnienie do nowej wersji dostawcy, aby uzyskać wszystkie ulepszenia. 

* EF Core, wraz z SQL Server i dostawcami znajdującymi się w pamięci, są dołączane do bieżących wersji ASP.NET Core. Aby uaktualnić istniejącą aplikację ASP.NET Core do nowszej wersji EF Core, należy zawsze uaktualnić wersję programu ASP.NET Core.

* Jeśli trzeba zaktualizować aplikację, która korzysta z dostawcy bazy danych innej firmy, należy zawsze sprawdzić, czy jest to aktualizacja dostawcy, która jest zgodna z wersją EF Core, która ma być używana. Na przykład dostawcy bazy danych dla poprzednich wersji nie są zgodni z wersją 2,0 środowiska uruchomieniowego EF Core.

* Dostawcy innych firm dla EF Core zwykle nie wydawania wersji poprawek wraz z EF Core środowiska uruchomieniowego. Aby uaktualnić aplikację, która używa dostawcy innej firmy do wersji poprawki EF Core, może być konieczne dodanie bezpośredniego odwołania do poszczególnych składników środowiska uruchomieniowego EF Core, takich jak Microsoft. EntityFrameworkCore i Microsoft. EntityFrameworkCore. relacyjny.

* W przypadku uaktualniania istniejącej aplikacji do najnowszej wersji EF Core niektóre odwołania do starszych pakietów EF Core mogą wymagać usunięcia ręcznie:

  * Pakiety czasu projektowania dostawcy bazy danych, takie `Microsoft.EntityFrameworkCore.SqlServer.Design` jak nie są już wymagane ani obsługiwane w EF Core 2,0 i nowszych, ale nie są automatycznie usuwane podczas uaktualniania innych pakietów.

  * Narzędzia interfejsu wiersza polecenia platformy .NET są dołączone do zestawu .NET SDK od wersji 2,1, więc odwołanie do tego pakietu można usunąć z pliku projektu:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

