---
title: Przenoszenie z EF6 do EF Core — przenoszenie modelu opartego na EDMX - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416924"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Przenoszenie modelu opartego na EF6 EDMX do ef core

EF Core nie obsługuje formatu pliku EDMX dla modeli. Najlepszym rozwiązaniem do portu tych modeli, jest wygenerowanie nowego modelu opartego na kodzie z bazy danych dla aplikacji.

## <a name="install-ef-core-nuget-packages"></a>Instalowanie pakietów EF Core NuGet

Zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakiet NuGet.

## <a name="regenerate-the-model"></a>Regeneracja modelu

Teraz można użyć funkcji inżynierii odwrotnej do utworzenia modelu na podstawie istniejącej bazy danych.

Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia – > Menedżer pakietów NuGet – > konsola Menedżera pakietów). Zobacz [Konsoli Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) dla opcji polecenia do szkieletu podzbiór tabel itp.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Na przykład w tym miejscu znajduje się polecenie szkieletu modelu z bazy danych blogów w wystąpieniu usługi SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Usuń model EF6

Można teraz usunąć model EF6 z aplikacji.

Jest w porządku, aby pozostawić ef6 NuGet pakiet (EntityFramework) zainstalowany, jak EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji. Jednak jeśli nie zamierzasz używać EF6 w żadnych obszarach aplikacji, a następnie odinstalowanie pakietu pomoże dać błędy kompilacji na kawałki kodu, które wymagają uwagi.

## <a name="update-your-code"></a>Aktualizowanie kodu

W tym momencie jest to kwestia adresowania błędów kompilacji i przeglądania kodu, aby sprawdzić, czy zmiany zachowania między EF6 i EF Core wpłynie na Ciebie.

## <a name="test-the-port"></a>Przetestuj port

Tylko dlatego, że aplikacja kompiluje, nie oznacza, że jest pomyślnie przeniesiony do EF Core. Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna ze zmian zachowania nie wpłynęła negatywnie na aplikację.
