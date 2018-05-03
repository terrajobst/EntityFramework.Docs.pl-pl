---
title: Eksportowanie z EF6 do Core EF — przenoszenie pliku EDMX, na podstawie modelu
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Eksportowanie EF6 EDMX na podstawie modelu EF Core

Podstawowe EF nie obsługuje formatu pliku EDMX dla modeli. Najlepszym rozwiązaniem do portu te modele jest aby wygenerować nowy model opartej na kodzie z bazy danych dla aplikacji.

## <a name="install-ef-core-nuget-packages"></a>Instalowanie pakietów EF Core NuGet

Zainstaluj `Microsoft.EntityFrameworkCore.Tools` pakietu NuGet.

## <a name="regenerate-the-model"></a>Ponowne generowanie modelu

Funkcje odtwarzania można teraz używać do utworzenia modelu na podstawie istniejącej bazy danych.

Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia –> Menedżera pakietów NuGet –> Konsola Menedżera pakietów). Zobacz [Konsola Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) dla opcji polecenia utworzyć szkielet podzbiór tabelach itp.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Na przykład w tym miejscu jest polecenie, aby utworzyć szkielet modelu z bazy danych w wystąpieniu programu SQL Server LocalDB obsługi blogów.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Usuń EF6 model

Teraz należy usunąć modelu EF6 z aplikacji.

Bardzo można pozostawić pakiet EF6 NuGet (EntityFramework), zainstalowane, EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji. Jednak jeśli nie są zamierza użyć EF6 w obszary aplikacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.

## <a name="update-your-code"></a>Zaktualizuj kod

W tym momencie jest kwestią adresowania błędy kompilacji i recenzowania kod, aby zobaczyć, czy zmiany zachowania między EF6 i podstawowe EF będzie miało wpływ na możesz.

## <a name="test-the-port"></a>Testowanie portu

Tak, ponieważ aplikacja kompiluje, oznacza to, że pomyślnie jest przenoszone na EF rdzeń. Należy przetestować wszystkie obszary w aplikacji w celu zapewnienia, że zmiany zachowania ma negatywny wpływ na aplikację.

> [!TIP]
> Zobacz [wprowadzenie EF rdzeni na platformy ASP.NET Core z istniejącej bazy danych](xref:core/get-started/aspnetcore/existing-db) uzyskać dodatkowe informacje na temat pracy z istniejącej bazy danych 
