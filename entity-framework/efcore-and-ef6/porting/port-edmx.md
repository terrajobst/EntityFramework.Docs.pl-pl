---
title: Przenoszenie z EF6 do EF Core-Porting model-Dr oparty na EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182064"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Przenoszenie modelu opartego na EF6 EDMX do EF Core

EF Core nie obsługuje formatu pliku EDMX dla modeli. Najlepszą opcją przenoszenia tych modeli jest generowanie nowego modelu opartego na kodzie z bazy danych aplikacji.

## <a name="install-ef-core-nuget-packages"></a>Zainstaluj EF Core pakiety NuGet

Zainstaluj pakiet NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Ponowne generowanie modelu

Teraz można użyć funkcji odtwarzania, aby utworzyć model oparty na istniejącej bazie danych.

Uruchom następujące polecenie w konsoli Menedżera pakietów (Narzędzia — > Menedżer pakietów NuGet — > konsoli Menedżera pakietów). Zobacz [konsola Menedżera pakietów (Visual Studio)](../../core/miscellaneous/cli/powershell.md) , aby wyświetlić opcje poleceń umożliwiające tworzenie szkieletu podzestawu tabel itp.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Na przykład poniżej przedstawiono polecenie do tworzenia szkieletu modelu z bazy danych blogów w wystąpieniu SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Usuń model EF6

Teraz można usunąć model EF6 z aplikacji.

Warto pozostawić zainstalowany pakiet NuGet EF6 (EntityFramework), ponieważ EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji. Jeśli jednak nie zamierzasz korzystać z EF6 w żadnych obszarach aplikacji, dezinstalacja pakietu pomoże Ci w wydaniu błędów kompilacji dla fragmentów kodu, które wymagają uwagi.

## <a name="update-your-code"></a>Aktualizowanie kodu

W tym momencie należy rozmieścić błędy kompilacji i przeglądać kod, aby sprawdzić, czy zachowanie zmienia się między EF6 a EF Core będzie miało wpływ na Ciebie.

## <a name="test-the-port"></a>Testowanie portu

Tak samo, jak Twoja aplikacja kompiluje, nie oznacza, że została pomyślnie przeniesiono do EF Core. Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna zmiana zachowania nie miała negatywnego wpływu na aplikację.
