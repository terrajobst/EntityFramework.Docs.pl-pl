---
title: "Informacje dotyczące wiersza polecenia - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
<a name="entity-framework-core-tools"></a>Entity Framework podstawowe narzędzia
===========================
Entity Framework podstawowe narzędzia pomóc podczas opracowywania aplikacji EF Core. Aby utworzyć szkielet typy DbContext i jednostki przez odtwarzanie schematu bazy danych oraz do zarządzania migracji są używane przede wszystkim.

[Konsoli Menedżera pakietów (PMC) EF podstawowe narzędzia] [ 1] zapewnienie wyższego poziomu środowiska w programie Visual Studio. Uruchom je przy użyciu narzędzia NuGet [Konsola Menedżera pakietów][2]. Te narzędzia Praca z projektami zarówno .NET Framework i .NET Core.

[Narzędzia wiersza polecenia platformy .NET Core EF] [ 3] są rozszerzeniem [narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core] [ 4] , które są obsługujący wiele platform i mogą uruchamiać poza Visual Studio. Narzędzia te wymagają projekt zestawu SDK programu .NET Core (jeden z `Sdk="Microsoft.NET.Sdk"` lub podobne w pliku projektu).

Zarówno narzędzia uwidaczniają te same funkcje. Jeśli projektujesz w programie Visual Studio, zaleca się za pomocą narzędzia PMC, ponieważ zapewniają większą integrację.

<a name="frameworks"></a>Struktury
----------
Narzędzia obsługi projektach przeznaczonych dla platformy .NET Framework lub .NET Core.

Jeśli chcesz użyć biblioteki klas, należy rozważyć, jeśli to możliwe przy użyciu biblioteki klas .NET Core lub .NET Framework. Spowoduje to co najmniej problemy z narzędzi platformy .NET. Jeśli zamiast tego chcesz użyć .NET Standard biblioteki klas, następnie należy użyć projekt startowy .NET Framework lub .NET Core tak, aby narzędzi conrete platformy docelowej, do którego można załadować biblioteki klas. Ten projekt startowy może być fikcyjny projektu z żadnego kodu rzeczywistych — jest wymagana jedynie zapewnienie elementu docelowego dla narzędzia.

Jeśli projekt jest przeznaczony dla innej framework (na przykład aplikacja uniwersalna systemu Windows lub Xamarin), następnie należy utworzyć oddzielne biblioteki klas .NET Standard. W takim przypadku należy wykonać wskazówki powyżej, aby również utworzyć projekt startowy, które mogą być używane przez narzędzia.

<a name="startup-and-target-projects"></a>Uruchamianie i projektów docelowych
---------------------------
Przy każdym wywołaniu polecenia obejmuje dwa projekty: Projekt startowy i projektu docelowego.

Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).

Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.

Zarówno projekt startowy, jak i docelowy projekt może być taka sama.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
