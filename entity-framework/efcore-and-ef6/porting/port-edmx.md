---
title: Przenoszenie z programu EF6 do programu EF Core — przenoszenie modelu opartego na EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997414"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Przenoszenie modelu opartego na EDMX EF6 do programu EF Core

EF Core nie obsługuje formatu pliku EDMX dla modeli. Najlepszym rozwiązaniem do portu tych modeli jest do generowania nowego modelu opartego na kodzie z bazy danych dla aplikacji.

## <a name="install-ef-core-nuget-packages"></a>Instalowanie pakietów programu EF Core NuGet

Zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakietu NuGet.

## <a name="regenerate-the-model"></a>Ponowne generowanie modelu

Funkcje odtwarzania można teraz używać do tworzenia modeli, w oparciu o istniejącą bazę danych.

Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia -> Menedżer pakietów NuGet -> Konsola Menedżera pakietów). Zobacz [Konsola Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) opcji polecenia do tworzenia szkieletu podzbioru tabel itp.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Na przykład poniżej przedstawiono polecenia do tworzenia szkieletu modelu z bazy danych do obsługi blogów w ramach wystąpienia programu SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Usuń EF6 model

EF6 model należy teraz usunąć z aplikacji.

Jest dobrym rozwiązaniem pozostawić pakiet NuGet platformy EF6 (EntityFramework), zainstalowane, zgodnie z programem EF Core i EF6 mogą być używane side-by-side w tej samej aplikacji. Jednak w przypadku korzystania z platformy EF6 w żadnych obszarów aplikacji nie są pomyślny przebieg operacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.

## <a name="update-your-code"></a>Aktualizowanie kodu

W tym momencie jest kwestią adresowania błędy kompilacji i recenzowania kod, aby sprawdzić, czy zmiany zachowania między EF6 i EF Core będzie miało wpływ na możesz.

## <a name="test-the-port"></a>Testowanie portu

Po prostu, ponieważ kompiluje aplikację, nie znaczy, że pomyślnie są przenoszone do programu EF Core. Należy przetestować wszystkie obszary w aplikacji, aby upewnić się, że żadne zmiany zachowania niekorzystnie wpłynąć na nie wpływ aplikacji.

> [!TIP]
> Zobacz [rozpoczęcie pracy z programem EF Core programu ASP.NET Core z istniejącej bazy danych](xref:core/get-started/aspnetcore/existing-db) Aby uzyskać dodatkowe informacje na temat sposobu pracy z istniejącej bazy danych 
