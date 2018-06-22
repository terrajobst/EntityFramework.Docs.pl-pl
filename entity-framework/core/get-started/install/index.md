---
title: Instalowanie EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054647"
---
# <a name="installing-ef-core"></a>Instalowanie EF Core

## <a name="prerequisites"></a>Wstępnie wymagane składniki

Aby tworzyć aplikacje .NET Core 2.0 (w tym aplikacje ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core), należy pobrać i zainstalować wersję [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) odpowiednie dla danej platformy. **Dotyczy to nawet po zainstalowaniu programu Visual Studio 2017 wersji 15 ustęp 3.**

Aby można było używać EF Core w wersji 2.0 lub inne biblioteki .NET 2.0 standardowe z platformami .NET, oprócz .NET Core 2.0 (np. z platformy .NET Framework 4.6.1 lub nowszej) będzie mieć wersję programu NuGet, który zna .NET 2.0 standardowe i jego struktury zgodne. Poniżej przedstawiono kilka sposobów, możesz uzyskać, to:

* Zainstaluj program Visual Studio 2017 wersji 15 ustęp 3
* Jeśli używasz programu Visual Studio 2015, [pobierania i uaktualnić klienta NuGet w wersji 3.6.0](https://www.nuget.org/downloads)

Projektów utworzonych w poprzednich wersjach programu Visual Studio i przeznaczonych dla platformy .NET Framework może być konieczne dodatkowe zmiany, aby był zgodny z bibliotekami .NET 2.0 standardowe:

* Edytuj plik projektu i upewnij się, że w grupie właściwości początkowej pojawia się następujący wpis:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Dla projektów testów upewnij się, że występuje następujący wpis:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Pobieranie usługi bits
Zalecanym sposobem Dodawanie bibliotek środowiska uruchomieniowego EF Core do aplikacji jest do zainstalowania dostawcy bazy danych programu EF Core z NuGet.

Oprócz bibliotek środowiska uruchomieniowego należy zainstalować narzędzia, które ułatwiają wykonać kilka zadań związanych z EF Core w projekcie w czasie projektowania, takie jak tworzenie i stosowanie migracji i tworzenia modelu na podstawie istniejącej bazy danych.

> [!TIP]  
> Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych z innych firm, zawsze Wyszukaj aktualizację dostawcy, który jest zgodny z wersją Core EF, którego chcesz użyć. Np. Baza danych dostawców w poprzednich wersjach nie są zgodne z EF podstawowego środowiska wykonawczego w wersji 2.0.  

> [!TIP]  
> Aplikacji przeznaczonych dla platformy ASP.NET Core 2.0 służy EF Core 2.0 bez dodatkowe zależności oprócz dostawcy bazy danych. Aplikacji na poprzednie wersje platformy ASP.NET Core trzeba uaktualnić do programu ASP.NET 2.0 Core aby można było używać EF Core 2.0.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Programowanie wieloplatformowych przy użyciu platformy .NET Core interfejsu wiersza polecenia (CLI)

Do tworzenia aplikacji przeznaczonych [.NET Core](https://www.microsoft.com/net/download/core) mogą być używane [ `dotnet` polecenia interfejsu wiersza polecenia](https://docs.microsoft.com/dotnet/core/tools/) w połączeniu z edytora tekstu lub programowanie środowiska IDE (Integrated) takie Program Visual Studio, programu Visual Studio for Mac lub kodu programu Visual Studio.

> [!IMPORTANT]  
> Aplikacji przeznaczonych dla platformy .NET Core wymagają określonych wersji programu Visual Studio, np. .NET Core 1.x programowanie wymaga programu Visual Studio 2017 r, podczas programowania .NET Core 2.0 wymaga programu Visual Studio 2017 wersji 15 ustęp 3.

Aby zainstalować lub uaktualnić dostawcę usługi programu SQL Server w aplikacji platformy .NET Core i platform, przełącz się do katalogu aplikacji i uruchom następujące polecenie w wierszu polecenia:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Można wskazać, instalowanie określonej wersji w `dotnet add package` polecenia przy użyciu `-v` modyfikator. Np. Aby zainstalować pakiety EF Core 2.0, dołącz `-v 2.0.0` do polecenia.

Podstawowe EF zawiera zestaw [dodatkowych poleceń dla `dotnet` CLI](../../miscellaneous/cli/dotnet.md)począwszy `dotnet ef`. Aby można było używać `dotnet ef` polecenia interfejsu wiersza polecenia aplikacji `.csproj` plik musi zawierać następujący wpis:

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

Narzędzia .NET Core CLI podstawowych EF również wymagać oddzielny pakiet o nazwie Microsoft.EntityFrameworkCore.Design. Można po prostu Dodaj do projektu przy użyciu:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Należy zawsze używać wersji pakiety narzędzia, które odpowiada wersji głównej pakietów środowiska wykonawczego.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Tworzenia programu Visual Studio

Możesz utworzyć wiele różnych typów aplikacji .NET Core obiektu docelowego, .NET Framework lub innych platform obsługiwanych przez EF Core za pomocą programu Visual Studio.

Istnieją dwa sposoby dostawcy bazy danych programu EF Core można zainstalować w aplikacji z programu Visual Studio:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Przy użyciu narzędzia NuGet [interfejsu użytkownika Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Wybierz z menu **projektu > Zarządzaj pakietami NuGet**

* Polecenie **Przeglądaj** lub **aktualizacje** kartę

* Wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i wymaganej wersji i upewnij się,

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Przy użyciu narzędzia NuGet [Konsola Menedżera pakietów (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* Wybierz z menu **Narzędzia > Menedżera pakietów NuGet > konsoli Menedżera pakietów**

* Wpisz i uruchom następujące polecenie w kryterium:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Można użyć `Update-Package` polecenia do zaktualizowania pakietu, który jest już zainstalowana na nowszą wersję

* Aby określić określonej wersji, można użyć `-Version` modyfikator, np. Aby zainstalować pakiety EF Core 2.0, dołącz `-Version 2.0.0` do poleceń

#### <a name="tools"></a>Narzędzia

Jest także wersja środowiska PowerShell [EF Core poleceń Uruchom, które wewnątrz PMC](../../miscellaneous/cli/powershell.md) w programie Visual Studio i podobnych funkcji `dotnet ef` poleceń. Aby można było ich używać, zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakietu przy użyciu interfejsu użytkownika Menedżera pakietów lub PMC.

> [!IMPORTANT]  
> Należy zawsze używać wersji pakiety narzędzia, które odpowiada wersji głównej pakietów środowiska wykonawczego.

> [!TIP]  
> Chociaż można używać `dotnet ef` poleceń PMC w programie Visual Studio jest znacznie bardziej wygodne korzysta z wersji programu PowerShell:
> * Automatycznie działają z bieżącego projektu wybranego w PMC bez konieczności ręcznego przełączania katalogów.  
> * Zostaną one automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.

> [!IMPORTANT]  
> **Przestarzałe pakietów w programie EF Core 2.0:** uaktualniania istniejącej aplikacji do EF Core 2.0 niektórych odwołań do starszych pakietów EF Core może muszą zostać usunięte ręcznie. W szczególności, takie jak bazy danych dostawcy czasu projektowania pakietów `Microsoft.EntityFrameworkCore.SqlServer.Design` już wymagane ani obsługiwane w programie EF Core 2.0, ale nie zostaną automatycznie usunięte podczas uaktualniania z innymi pakietami.
