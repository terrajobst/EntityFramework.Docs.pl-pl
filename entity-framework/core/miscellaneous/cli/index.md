---
title: Informacje dotyczące wiersza polecenia — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490366"
---
<a name="entity-framework-core-tools"></a>Entity Framework Core Tools
===========================
Entity Framework Core Tools pomocne podczas tworzenia aplikacji programu EF Core. Służą one głównie do tworzenia szkieletu typu DbContext i jednostki przez odtwarzanie schemat bazy danych oraz do zarządzania migracji.

[Narzędzia konsoli Menedżera pakietów (PMC) EF Core] [ 1] zapewniając doskonałe środowisko w programie Visual Studio. Uruchamiaj je za pomocą NuGet [Konsola Menedżera pakietów][2]. Te narzędzia działają w projektach .NET Core i .NET Framework.

[Narzędzia wiersza polecenia platformy .NET Core EF] [ 3] stanowią rozszerzenie do [narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core] [ 4] , które są dla wielu platform i mogą być uruchamiane poza programem Visual Studio. Narzędzia te wymagają projektu .NET Core SDK (z `Sdk="Microsoft.NET.Sdk"` lub podobne w pliku projektu).

Oba narzędzia uwidaczniają taką samą funkcjonalność. Jeśli tworzysz w programie Visual Studio, zaleca się przy użyciu narzędzi konsolę zarządzania Pakietami, ponieważ zapewniają one bardziej zintegrowanego środowiska pracy.

<a name="frameworks"></a>Struktury
----------
Narzędzia obsługują projekty przeznaczone dla .NET Framework lub .NET Core.

Jeśli chcesz użyć biblioteki klas, następnie należy wziąć pod uwagę przy użyciu biblioteki klas platformy .NET Core lub .NET Framework, jeśli jest to możliwe. Spowoduje to co najmniej problemów za pomocą narzędzi platformy .NET. Jeśli zamiast tego chcesz użyć biblioteki klas .NET Standard, następnie należy użyć projekt startowy platformy .NET Framework lub .NET Core tak, aby narzędzi conrete platformę docelową, do którego można załadować biblioteki klas. Ten projekt startowy może być fikcyjnego projektu bez rzeczywistego kodu — jest to tylko potrzebne do udostępniać miejsce docelowe dla narzędzi.

Jeśli projekt jest przeznaczony dla innej framework (na przykład, Universal Windows lub platformy Xamarin), następnie należy utworzyć oddzielne biblioteki klas .NET Standard. W takim przypadku postępuj zgodnie z wskazówki powyżej, aby również utworzyć projekt startowy, który może być używany przez narzędzi.

<a name="startup-and-target-projects"></a>Uruchamianie i projektów docelowych
---------------------------
Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty: projekt docelowy i projekt startowy.

Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte).

Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.

Zarówno projekt docelowy, jak i projekt startowy może być taki sam.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
