---
title: "Informacje dotyczące wiersza polecenia - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
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

Jeśli projekt jest przeznaczony dla innej framework (na przykład aplikacja uniwersalna systemu Windows lub Xamarin), zaleca się utworzenie oddzielnej .NET Standard projektów i elementów docelowych między jedną z obsługiwanych platform.

Docelowe między .NET Core, na przykład, kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj \*.csproj**. Aktualizacja `TargetFramework` właściwości w następujący sposób. (Należy pamiętać, nazwa właściwości staje się w liczbie mnogiej.)

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Jeśli używasz programu .NET Standard biblioteki klas, nie trzeba między docelowych, jeśli Twój projekt startowy jest przeznaczony dla platformy .NET Framework lub .NET Core.

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
