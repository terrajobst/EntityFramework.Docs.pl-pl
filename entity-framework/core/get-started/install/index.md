---
title: Instalowanie rdzenia entity framework — EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 6575b1ac028f8b67b49ca7f4e49d6f19500be98f
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136171"
---
# <a name="installing-entity-framework-core"></a>Instalowanie rdzenia struktury encji

## <a name="prerequisites"></a>Wymagania wstępne

* EF Core jest biblioteką [.NET Standard 2.0.](/dotnet/standard/net-standard) Tak EF Core wymaga implementacji .NET, która obsługuje .NET Standard 2.0 do uruchomienia. EF Core może również odwoływać się do innych bibliotek .NET Standard 2.0.

* Na przykład można użyć EF Core do tworzenia aplikacji, które są przeznaczone dla .NET Core. Tworzenie aplikacji .NET Core wymaga [sdk .NET Core.](https://dotnet.microsoft.com/download) Opcjonalnie można również użyć środowiska programistycznego, takiego jak [Visual Studio,](https://visualstudio.microsoft.com/vs) [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac)lub Visual Studio [Code.](https://code.visualstudio.com) Aby uzyskać więcej informacji, zapoznaj [się z polem zapoznawym Wprowadzenie do platformy .NET Core](/dotnet/core/get-started).

* Ef Core służy do tworzenia aplikacji w systemie Windows przy użyciu programu Visual Studio. Zalecana jest najnowsza wersja [programu Visual Studio.](https://visualstudio.microsoft.com/vs)

* EF Core można uruchomić w innych implementacjach platformy .NET, takich jak [Xamarin](https://dotnet.microsoft.com/apps/xamarin) i .NET Native. Ale w praktyce te implementacje mają ograniczenia środowiska uruchomieniowego, które mogą mieć wpływ na to, jak dobrze EF Core działa w aplikacji. Aby uzyskać więcej informacji, zobacz [implementacje platformy .NET obsługiwane przez EF Core](xref:core/platforms/index).

* Na koniec różnych dostawców baz danych może wymagać określonych wersji aparatu bazy danych, .NET implementacji lub systemów operacyjnych. Upewnij się, że [dostawca bazy danych EF Core](xref:core/providers/index) jest dostępny, który obsługuje odpowiednie środowisko dla aplikacji.

## <a name="get-the-entity-framework-core-runtime"></a>Pobierz środowisko uruchomieniowe Core frameworka entity framework

Aby dodać EF Core do aplikacji, zainstaluj pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.

Jeśli budujesz aplikację ASP.NET Core, nie musisz instalować dostawców w pamięci i programu SQL Server. Dostawcy ci są zawarte w bieżących wersjach ASP.NET Core, obok środowiska uruchomieniowego EF Core.  

Aby zainstalować lub zaktualizować pakiety NuGet, można użyć interfejsu wiersza polecenia .NET Core (CLI), okna dialogowego Menedżera pakietów programu Visual Studio lub konsoli menedżera pakietów programu Visual Studio.

### <a name="net-core-cli"></a>Interfejs wiersza polecenia platformy .NET Core

* Użyj następującego polecenia .NET Core CLI z wiersza polecenia systemu operacyjnego, aby zainstalować lub zaktualizować dostawcę programu EF Core SQL Server:

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Można wskazać określoną wersję `dotnet add package` w poleceniu, używając modyfikatora. `-v` Na przykład, aby zainstalować pakiety EF Core 2.2.0, dołącz `-v 2.2.0` do polecenia.

Aby uzyskać więcej informacji, zobacz [narzędzia interfejsu wiersza polecenia .NET .](/dotnet/core/tools/)

### <a name="visual-studio-nuget-package-manager-dialog"></a>Okno dialogowe Menedżera pakietów programu Visual Studio NuGet

* Z menu Visual Studio wybierz polecenie **Project > Manage NuGet Packages**

* Kliknij kartę **Przeglądaj** lub **Aktualizacje**

* Aby zainstalować lub zaktualizować dostawcę `Microsoft.EntityFrameworkCore.SqlServer` programu SQL Server, wybierz pakiet i potwierdź.

Aby uzyskać więcej informacji, zobacz [Okno dialogowe Menedżera pakietów NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konsola menedżera pakietów programu Visual Studio NuGet

* Z menu Programu Visual Studio wybierz polecenie **Narzędzia > Menedżer pakietów NuGet > konsoli Menedżera pakietów**

* Aby zainstalować dostawcę programu SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Aby zaktualizować dostawcę, `Update-Package` użyj polecenia.

* Aby określić określoną `-Version` wersję, należy użyć modyfikatora. Na przykład, aby zainstalować pakiety EF Core 2.2.0, dołącz `-Version 2.2.0` do poleceń

Aby uzyskać więcej informacji, zobacz [Konsola Menedżera pakietów](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Pobierz narzędzia Core struktury encji

Można zainstalować narzędzia do wykonywania zadań związanych z EF Core w projekcie, takich jak tworzenie i stosowanie migracji bazy danych lub tworzenie modelu EF Core na podstawie istniejącej bazy danych.

Dostępne są dwa zestawy narzędzi:

* Narzędzia [interfejsu wiersza polecenia .NET Core (CLI)](xref:core/miscellaneous/cli/dotnet) mogą być używane w systemach Windows, Linux lub macOS. Te polecenia zaczynają `dotnet ef`się od .

* Narzędzia [konsoli menedżera pakietów (PMC)](xref:core/miscellaneous/cli/powershell) są uruchamiane w programie Visual Studio w systemie Windows. Te polecenia zaczynają się od czasownika, na przykład `Add-Migration`. `Update-Database`

Chociaż można również `dotnet ef` użyć poleceń z konsoli Menedżera pakietów, zaleca się używanie narzędzi Konsoli Menedżera pakietów podczas korzystania z programu Visual Studio:

* Automatycznie działają z bieżącym projektem wybranym w pmc w programie Visual Studio, bez konieczności ręcznego przełączania katalogów.  

* Automatycznie otwierają pliki generowane przez polecenia w programie Visual Studio po zakończeniu polecenia.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Pobierz narzędzia .NET Core CLI

Narzędzia .NET Core CLI wymagają zestawu .NET Core SDK, o którym mowa wcześniej w [wymaganiach wstępnych](#prerequisites).

Polecenia `dotnet ef` są zawarte w bieżących wersjach zestawu .NET Core SDK, ale aby włączyć polecenia `Microsoft.EntityFrameworkCore.Design` w określonym projekcie, należy zainstalować pakiet:

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]
> Zawsze używaj wersji pakietu narzędzi, która pasuje do głównej wersji pakietów środowiska wykonawczego.

### <a name="get-the-package-manager-console-tools"></a>Pobierz narzędzia konsoli Menedżera pakietów

Aby uzyskać narzędzia konsoli Menedżera pakietów `Microsoft.EntityFrameworkCore.Tools` dla EF Core, zainstaluj pakiet. Na przykład z programu Visual Studio:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

W przypadku aplikacji ASP.NET Core ten pakiet jest dołączony automatycznie.

## <a name="upgrading-to-the-latest-ef-core"></a>Uaktualnienie do najnowszego ef core

* Za każdym razem, gdy wydajemy nową wersję ef core, możemy również zwolnić nową wersję dostawców, które są częścią projektu EF Core, takich jak Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite i Microsoft.EntityFrameWorkCore.InMemory. Możesz po prostu uaktualnić do nowej wersji dostawcy, aby uzyskać wszystkie ulepszenia.

* EF Core, wraz z SQL Server i dostawców w pamięci są zawarte w bieżących wersjach ASP.NET Core. Aby uaktualnić istniejącą aplikację ASP.NET Core do nowszej wersji ef core, należy zawsze uaktualnić wersję ASP.NET Core.

* Jeśli chcesz zaktualizować aplikację, która korzysta z dostawcy bazy danych innej firmy, zawsze sprawdź, czy jest dostępna aktualizacja dostawcy, która jest zgodna z wersją ef core, którego chcesz użyć. Na przykład dostawcy baz danych dla poprzednich wersji nie są zgodne z wersją 2.0 środowiska wykonawczego EF Core.

* Zewnętrzni dostawcy ef core zwykle nie zwolnić wersje poprawek wraz ze środowiska uruchomieniowego EF Core. Aby uaktualnić aplikację, która używa dostawcy zewnętrznego do wersji poprawki EF Core, może być konieczne dodanie bezpośredniego odwołania do poszczególnych składników środowiska wykonawczego EF Core, takich jak Microsoft.EntityFrameworkCore i Microsoft.EntityFrameworkCore.Relational.

* Jeśli uaktualniasz istniejącą aplikację do najnowszej wersji EF Core, niektóre odwołania do starszych pakietów EF Core mogą wymagać ręcznego usunięcia:

  * Pakiety czasu projektowania `Microsoft.EntityFrameworkCore.SqlServer.Design` dostawcy bazy danych, takie jak nie są już wymagane lub obsługiwane z EF Core 2.0 i nowsze, ale nie są automatycznie usuwane podczas uaktualniania innych pakietów.

  * Narzędzia interfejsu wiersza polecenia platformy .NET są zawarte w zestawie .NET SDK od wersji 2.1, więc odwołanie do tego pakietu można usunąć z pliku projektu:

    ``` xml
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```
