---
title: Instalowanie programu EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 7bb2ee11940a4fd5736c7a23c16533ef53018f7b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949195"
---
# <a name="installing-ef-core"></a>Instalowanie programu EF Core

## <a name="prerequisites"></a>Wymagania wstępne

W celu opracowywania aplikacji platformy .NET Core 2.0 (w tym aplikacji ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core), należy pobrać i zainstalować wersję [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) odpowiednie dla danej platformy. **Ta zasada obowiązuje, nawet jeśli zainstalowano program Visual Studio 2017 w wersji 15.3.**

Aby można było używać programu EF Core 2.0 lub inne biblioteki .NET Standard 2.0 przy użyciu platformy .NET, oprócz .NET Core 2.0 (na przykład za pomocą programu .NET Framework 4.6.1 lub nowszej) potrzebna jest wersja pakietu nuget, rozpoznający .NET Standard 2.0 i jego struktury zgodne. Oto kilka sposobów, można uzyskać, to:

* Instalowanie programu Visual Studio 2017 w wersji 15.3
* Jeśli używasz programu Visual Studio 2015 [pobierania i uaktualnić klienta programu NuGet do wersji 3.6.0](https://www.nuget.org/downloads)

Projekty utworzone we wcześniejszych wersjach programu Visual Studio i przeznaczonych dla platformy .NET Framework może być konieczne dodatkowe zmiany, aby był zgodny z biblioteki .NET Standard 2.0:

* Edytuj plik projektu i upewnij się, że w grupie właściwości początkowe pojawia się następujący wpis:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Dla projektów testowych również upewnij się, że występuje następujący wpis:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Pobieranie usługi bits
Zalecanym sposobem dodawania bibliotek środowiska uruchomieniowego EF Core w aplikacji jest również zainstalować dostawcy bazy danych programu EF Core z pakietów NuGet.

Oprócz bibliotek środowiska uruchomieniowego można zainstalować narzędzia, które ułatwiają wykonać kilka zadań związanych z programem EF Core w projekcie, w czasie projektowania, takie jak tworzenie i stosowanie migracje i tworzenie modelu, w oparciu o istniejącą bazę danych.

> [!TIP]  
> Jeśli musisz zaktualizować aplikację, która używa dostawcy bazy danych innej firmy, Zawsze sprawdzaj dostępność aktualizacji dostawcy, który jest zgodny z wersją programu EF Core, którego chcesz użyć. Na przykład dostawcy baz danych dla wcześniejszych wersji nie są zgodne z wersji 2.0 środowiska uruchomieniowego EF Core.  

> [!TIP]  
> Aplikacje przeznaczone na platformy ASP.NET Core 2.0 mogą używać programu EF Core 2.0 bez dodatkowe zależności oprócz dostawcy bazy danych. Aplikacje przeznaczone na poprzednie wersje platformy ASP.NET Core konieczne uaktualnienie do programu ASP.NET Core 2.0, aby można było używać programu EF Core 2.0.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Programowanie dla wielu platform przy użyciu konsoli .NET Core interfejsu wiersza polecenia (CLI)

Do tworzenia aplikacji, których platformą docelową [platformy .NET Core](https://www.microsoft.com/net/download/core) możesz użyć [ `dotnet` poleceń interfejsu wiersza polecenia](https://docs.microsoft.com/dotnet/core/tools/) w połączeniu z ulubionego edytora tekstu lub tworzenia środowiska IDE (Integrated) takie Program Visual Studio, Visual Studio for Mac lub Visual Studio Code.

> [!IMPORTANT]  
> Aplikacji przeznaczonych dla platformy .NET Core wymagają określonych wersji programu Visual Studio. Na przykład .NET Core 1.x rozwoju wymaga programu Visual Studio 2017, podczas programowania .NET Core 2.0 wymaga programu Visual Studio 2017 w wersji 15.3.

Aby zainstalować lub uaktualnić dostawcę programu SQL Server w aplikacji platformy .NET Core dla wielu platform, przejdź do katalogu aplikacji, a następnie uruchom następujące polecenie w wierszu polecenia:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Można wskazać instalowanie określonej wersji, w `dotnet add package` polecenia przy użyciu `-v` modyfikator. Na przykład, aby zainstalować pakiety programu EF Core 2.0, należy dołączyć `-v 2.0.0` do polecenia.

EF Core obejmuje zestaw [dodatkowe polecenia do `dotnet` interfejsu wiersza polecenia](../../miscellaneous/cli/dotnet.md), począwszy od `dotnet ef`. Aby można było używać `dotnet ef` poleceń interfejsu wiersza polecenia, Twoja aplikacja `.csproj` plik musi zawierać następujący wpis:

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

Narzędzia wiersza polecenia platformy .NET Core dla platformy EF Core wymagają również oddzielny pakiet o nazwie Microsoft.EntityFrameworkCore.Design. Można po prostu dodaj go do projektu za pomocą:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Zawsze używaj wersji pakietów narzędzi, które są zgodne z wersją główną pakiety środowiska uruchomieniowego.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Programowanie Visual Studio

Możesz tworzyć wiele różnych typów aplikacji, których platformą docelową .NET Core, .NET Framework lub na innych platformach obsługiwanych przez platformę EF Core przy użyciu programu Visual Studio.

Istnieją dwa sposoby, można zainstalować dostawcy bazy danych programu EF Core w aplikacji z programu Visual Studio:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Za pomocą NuGet [interfejsu użytkownika Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* Wybierz z menu **Projekt > Zarządzaj pakietami NuGet**

* Kliknij pozycję **Przeglądaj** lub **aktualizacje** kartę

* Wybierz `Microsoft.EntityFrameworkCore.SqlServer` pakietu i wymaganej wersji i upewnij się,

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Za pomocą NuGet [Konsola Menedżera pakietów (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* Wybierz z menu **Narzędzia > Menedżer pakietów NuGet > Konsola Menedżera pakietów**

* Wpisz i uruchom następujące polecenie w konsoli zarządzania Pakietami:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Możesz użyć `Update-Package` zamiast tego polecenia, aby zaktualizować pakiet, który jest już zainstalowany na nowszą wersję

* Aby określić określonej wersji, można użyć `-Version` modyfikator. Na przykład, aby zainstalować pakiety programu EF Core 2.0, należy dołączyć `-Version 2.0.0` poleceń

#### <a name="tools"></a>Narzędzia

Jest również wersja programu PowerShell z [programu EF Core polecenia uruchomienia, które w konsoli zarządzania Pakietami](../../miscellaneous/cli/powershell.md) w programie Visual Studio, podobne możliwości `dotnet ef` poleceń. Aby można było ich użyć, należy zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu przy użyciu interfejsu użytkownika Menedżera pakietów lub konsoli zarządzania Pakietami.

> [!IMPORTANT]  
> Zawsze używaj wersji pakietów narzędzi, które są zgodne z wersją główną pakiety środowiska uruchomieniowego.

> [!TIP]  
> Chociaż istnieje możliwość użycia `dotnet ef` polecenia z konsoli zarządzania Pakietami w programie Visual Studio, jest to o wiele bardziej wygodne do korzystania z wersji programu PowerShell:
> * Działają automatycznie z bieżącego projektu wybrany w konsoli zarządzania Pakietami, bez konieczności ręcznie zmienić katalog.  
> * One automatycznie otwarte pliki wygenerowane za pomocą poleceń w programie Visual Studio po zakończeniu polecenia.

> [!IMPORTANT]  
> **Przestarzałe pakiety w programie EF Core 2.0:** uaktualniania istniejącej aplikacji do programu EF Core 2.0, niektórych odwołań do starszych pakietów programu EF Core może być konieczne ręczne usunięcie. W szczególności, takich jak bazy danych dostawcy projektowania pakietów `Microsoft.EntityFrameworkCore.SqlServer.Design` nie jest już wymagane lub obsługiwanych w programie EF Core 2.0, ale nie zostaną automatycznie usunięte po uaktualnieniu innych pakietów.
