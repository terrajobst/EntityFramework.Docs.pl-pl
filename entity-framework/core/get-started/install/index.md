---
title: Instalowanie platformy Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 5ebc4edba07063ad5e77154adcde5f2664c0d748
ms.sourcegitcommit: 85d17524d8e022f933cde7fc848313f57dfd3eb8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55760525"
---
# <a name="installing-entity-framework-core"></a>Instalowanie platformy Entity Framework Core

## <a name="prerequisites"></a>Wymagania wstępne

* EF Core jest [.NET Standard 2.0](/dotnet/standard/net-standard) biblioteki. EF Core wymaga implementacji .NET, która obsługuje .NET Standard 2.0 do uruchomienia. EF Core również mogą być przywoływane przez inne biblioteki .NET Standard 2.0. 

* Na przykład można użyć programu EF Core programować aplikacje, których platformą docelową .NET Core. Tworzenie aplikacji .NET Core wymaga [zestawu .NET Core SDK](https://dotnet.microsoft.com/download). Opcjonalnie umożliwia także środowiska programowania, takich jak Visual Studio, Visual Studio for Mac lub Visual Studio Code. Aby uzyskać więcej informacji, sprawdź [wprowadzenie do platformy .NET Core](/dotnet/core/get-started).

* EF Core służy do tworzenia aplikacji, których platformą docelową .NET Framework 4.6.1 lub później na Windows, za pomocą programu Visual Studio. Najnowszą wersję programu Visual Studio jest zalecane. Jeśli chcesz używać starszej wersji, takich jak Visual Studio 2015, upewnij się, że [uaktualnić klienta programu NuGet w wersji 3.6.0](https://www.nuget.org/downloads) do pracy z biblioteki .NET Standard 2.0.

* EF Core można uruchamiać na inne implementacje platformy .NET, takich jak Xamarin i .NET Native. Ale w praktyce te implementacje ograniczenia środowiska uruchomieniowego, które mogą dotyczyć, jak dobrze działa programu EF Core w aplikacji. Aby uzyskać więcej informacji, zobacz [implementacji platformy .NET obsługiwanych przez platformę EF Core](xref:core/platforms/index).

* Na koniec dostawców innej bazy danych może wymagać konkretnej bazy danych aparatu wersje, implementacje platformy .NET lub systemów operacyjnych. Upewnij się, że [dostawcy bazy danych programu EF Core](xref:core/providers/index) jest dostępna, która obsługuje odpowiednim środowisku dla danej aplikacji.

## <a name="get-the-entity-framework-core-runtime"></a>Pobierz środowisko uruchomieniowe programu Entity Framework Core

Do programu EF Core można dodać do aplikacji, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.

Jeśli tworzysz aplikację ASP.NET Core, nie trzeba zainstalować w pamięci i dostawców programu SQL Server. Te dostawcy są uwzględnieni w bieżących wersjach platformy ASP.NET Core, wraz z środowiska uruchomieniowego EF Core.  

Aby zainstalować lub zaktualizować pakiety NuGet, można użyć platformy .NET Core interfejsu wiersza polecenia (CLI), okno Menedżera pakietów Visual Studio lub konsoli Menedżera pakietów Visual Studio.

### <a name="net-core-cli"></a>.NET core interfejsu wiersza polecenia

* Użyj następującego polecenia interfejsu wiersza polecenia platformy .NET Core z wiersza polecenia systemu operacyjnego, aby zainstalować lub zaktualizować dostawcy EF Core programu SQL Server:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Można wskazać określonej wersji w `dotnet add package` polecenia przy użyciu `-v` modyfikator. Na przykład, aby zainstalować pakiety programu EF Core 2.2.0, należy dołączyć `-v 2.2.0` do polecenia.

Aby uzyskać więcej informacji, zobacz [narzędzia interfejsu wiersza polecenia (CLI) platformy .NET](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Okno Menedżera pakietów NuGet programu Visual Studio

* Wybierz z menu programu Visual Studio **Projekt > Zarządzaj pakietami NuGet**

* Kliknij pozycję **Przeglądaj** lub **aktualizacje** kartę

* Aby zainstalować lub zaktualizować dostawcy programu SQL Server, wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i potwierdź.

Aby uzyskać więcej informacji, zobacz [okno Menedżera pakietów NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Konsola Menedżera pakietów NuGet dla programu Visual Studio

* Wybierz z menu programu Visual Studio **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Aby zainstalować dostawcę programu SQL Server, uruchom następujące polecenie w konsoli Menedżera pakietów:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Aby zaktualizować dostawcę, użyj `Update-Package` polecenia.

* Aby określić określonej wersji, użyj `-Version` modyfikator. Na przykład, aby zainstalować pakiety programu EF Core 2.2.0, należy dołączyć `-Version 2.2.0` poleceń

Aby uzyskać więcej informacji, zobacz [Konsola Menedżera pakietów](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Pobierz narzędzia platformy Entity Framework Core

Możesz zainstalować narzędzia do wykonywania zadań związanych z programem EF Core w projekcie, takich jak tworzenie i stosowanie migracje baz danych lub tworzenie modelu platformy EF Core oparte na istniejącej bazy danych.

Dostępne są dwa zestawy narzędzi:

* [Narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core](xref:core/miscellaneous/cli/dotnet) może służyć w Windows, Linux lub macOS. Te polecenia zaczynają się od `dotnet ef`. 

* [Narzędzia konsoli Menedżera pakietów (PMC)](xref:core/miscellaneous/cli/powershell) systemem Windows w programie Visual Studio. Te polecenia rozpoczynać czasownika, na przykład `Add-Migration`, `Update-Database`.

Mimo że można również użyć `dotnet ef` poleceń z poziomu konsoli Menedżera pakietów, zaleca się korzystania z narzędzi Konsola Menedżera pakietów, podczas korzystania z programu Visual Studio:

* Działają automatycznie z bieżącego projektu wybrany w konsoli zarządzania Pakietami w programie Visual Studio, bez konieczności ręcznie zmienić katalog.  

* One automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Pobierz narzędzia wiersza polecenia platformy .NET Core

Narzędzia interfejsu wiersza polecenia platformy .NET core wymagają .NET Core SDK, wymienione wcześniej w [wymagania wstępne](#prerequisites).

`dotnet ef` Polecenia są zawarte w aktualnych wersjach programu .NET Core SDK, ale aby włączyć polecenia dla danego projektu, musisz zainstalować `Microsoft.EntityFrameworkCore.Design` pakietu:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

W przypadku aplikacji platformy ASP.NET Core, ten pakiet jest uwzględniane automatycznie.

> [!IMPORTANT]      
> Zawsze używaj wersji zgodnej wersji głównej pakiety środowiska uruchomieniowego pakietu Narzędzia.

### <a name="get-the-package-manager-console-tools"></a>Pobierz narzędzia w konsoli Menedżera pakietów

Aby uzyskać Konsola Menedżera pakietów narzędzi dla platformy EF Core, należy zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu. Na przykład z programu Visual Studio:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

W przypadku aplikacji platformy ASP.NET Core, ten pakiet jest uwzględniane automatycznie.

## <a name="upgrading-to-the-latest-ef-core"></a>Uaktualnianie do najnowszej EF Core

* Każdym wydaniu nowej wersji programu EF Core, wydaniu nowej wersji dostawców, które są częścią projektu programu EF Core, takich jak Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, i Microsoft.EntityFrameworkCore.InMemory. Możesz po prostu uaktualnić do nowej wersji dostawcy, aby uzyskać wszystkie ulepszenia. 

* EF Core wraz z programu SQL Server i dostawców w pamięci są uwzględnione w aktualnych wersjach programu ASP.NET Core. Aby przeprowadzić uaktualnienie do nowszej wersji programu EF Core istniejącej aplikacji platformy ASP.NET Core, zawsze należy uaktualnić wersję platformy ASP.NET Core.

* Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych innej firmy, Zawsze sprawdzaj dostępność aktualizacji dostawcy, który jest zgodny z wersją programu EF Core, którego chcesz użyć. Na przykład dostawcy baz danych dla wcześniejszych wersji nie są zgodne z wersji 2.0 środowiska uruchomieniowego EF Core.

* Innych dostawców dla platformy EF Core zwykle nie zwalniaj wersji poprawki wraz z środowiska uruchomieniowego EF Core. Aby uaktualnić aplikację, która używa dostawcy innej firmy, do wersji poprawki programu EF Core, może być konieczne dodanie bezpośrednie odwołanie do poszczególnych składników środowiska uruchomieniowego EF Core, takie jak Microsoft.EntityFrameworkCore i Microsoft.EntityFrameworkCore.Relational.

* Jeśli aktualizujesz istniejącą aplikację do najnowszej wersji programu EF Core niektórych odwołań do starszych pakietów programu EF Core może być konieczne ręczne usunięcie:

  * Bazy danych dostawcy projektowania pakietów, takich jak `Microsoft.EntityFrameworkCore.SqlServer.Design` nie są już wymagane lub obsługiwane z programem EF Core 2.0 lub nowszy, ale nie są automatycznie usuwane po uaktualnieniu innych pakietów.

  * Narzędzia wiersza polecenia platformy .NET obejmuje zestawu .NET SDK od wersji 2.1, dzięki czemu można usunąć odwołania do tego pakietu z pliku projektu:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* Aplikacji przeznaczonych dla środowiska .NET Framework może być konieczne zmiany, aby pracować z biblioteki .NET Standard 2.0:

  * Edytuj plik projektu i upewnij się, że w grupie właściwości początkowe pojawia się następujący wpis:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Dla projektów testowych również upewnij się, że występuje następujący wpis:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
