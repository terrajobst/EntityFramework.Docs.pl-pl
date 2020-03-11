---
title: Wersje programu Visual Studio — EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416941"
---
# <a name="visual-studio-releases"></a>Wydania programu Visual Studio

Zalecamy, aby zawsze używać najnowszej wersji programu Visual Studio, ponieważ zawiera ona najnowsze narzędzia dla platform .NET, NuGet i Entity Framework.
W rzeczywistości różne przykłady i instruktaże w dokumentacji Entity Framework założono, że używasz najnowszej wersji programu Visual Studio.

Istnieje jednak możliwość użycia starszych wersji programu Visual Studio z różnymi wersjami Entity Framework, o ile uwzględniono pewne różnice:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 i nowsze

- Ta wersja programu Visual Studio zawiera najnowszą wersję narzędzi Entity Framework oraz środowisko uruchomieniowe EF 6,2 i nie wymaga dodatkowych kroków konfiguracyjnych.
Zobacz, [co nowego](~/ef6/what-is-new/index.md) , aby uzyskać więcej informacji dotyczących tych wersji.
- Dodanie Entity Framework do nowych projektów przy użyciu narzędzi EF spowoduje automatyczne dodanie pakietu NuGet Dr 6,2.
Możesz ręcznie zainstalować lub uaktualnić wszystkie pakiety NuGet EF dostępne online.
- Domyślnie wystąpienie SQL Server dostępne w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie MSSQLLocalDB.
Sekcja serwera parametrów połączenia, których należy użyć, to "(LocalDB)\\MSSQLLocalDB".
Należy pamiętać, aby użyć ciągu Verbatim poprzedzonego prefiksem `@` lub podwójnym ukośnikiem "\\\\" podczas określania parametrów połączenia w C# kodzie.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Program Visual Studio 2015 do programu Visual Studio 2017 15,6

- Te wersje programu Visual Studio obejmują narzędzia Entity Framework i środowiska uruchomieniowe programu 6.1.3.
Więcej informacji o tych wersjach można znaleźć w [poprzednich wersjach](~/ef6/what-is-new/past-releases.md#ef-613) .
- Dodanie Entity Framework do nowych projektów przy użyciu narzędzi EF spowoduje automatyczne dodanie pakietu NuGet programu EF 6.1.3.
Możesz ręcznie zainstalować lub uaktualnić wszystkie pakiety NuGet EF dostępne online.
- Domyślnie wystąpienie SQL Server dostępne w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie MSSQLLocalDB.
Sekcja serwera parametrów połączenia, których należy użyć, to "(LocalDB)\\MSSQLLocalDB".
Należy pamiętać, aby użyć ciągu Verbatim poprzedzonego prefiksem `@` lub podwójnym ukośnikiem "\\\\" podczas określania parametrów połączenia w C# kodzie.  


## <a name="visual-studio-2013"></a>Program Visual Studio 2013
- Ta wersja programu Visual Studio zawiera i starszą wersję narzędzi Entity Framework i środowiska uruchomieniowego.
Zaleca się uaktualnienie do Entity Framework Tools 6.1.3 przy użyciu [Instalatora](https://www.microsoft.com/download/details.aspx?id=40762) dostępnego w centrum pobierania Microsoft.
Więcej informacji o tych wersjach można znaleźć w [poprzednich wersjach](~/ef6/what-is-new/past-releases.md#ef-613) .
- Dodanie Entity Framework do nowych projektów przy użyciu uaktualnionych narzędzi EF spowoduje automatyczne dodanie pakietu NuGet programu EF 6.1.3.
Możesz ręcznie zainstalować lub uaktualnić wszystkie pakiety NuGet EF dostępne online.
- Domyślnie wystąpienie SQL Server dostępne w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie MSSQLLocalDB.
Sekcja serwera parametrów połączenia, których należy użyć, to "(LocalDB)\\MSSQLLocalDB".
Należy pamiętać, aby użyć ciągu Verbatim poprzedzonego prefiksem `@` lub podwójnym ukośnikiem "\\\\" podczas określania parametrów połączenia w C# kodzie.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Ta wersja programu Visual Studio zawiera i starszą wersję narzędzi Entity Framework i środowiska uruchomieniowego.
Zaleca się uaktualnienie do Entity Framework Tools 6.1.3 przy użyciu [Instalatora](https://www.microsoft.com/download/details.aspx?id=40762) dostępnego w centrum pobierania Microsoft.
Więcej informacji o tych wersjach można znaleźć w [poprzednich wersjach](~/ef6/what-is-new/past-releases.md#ef-613) .
- Dodanie Entity Framework do nowych projektów przy użyciu uaktualnionych narzędzi EF spowoduje automatyczne dodanie pakietu NuGet programu EF 6.1.3.
Możesz ręcznie zainstalować lub uaktualnić wszystkie pakiety NuGet EF dostępne online.
- Domyślnie wystąpienie SQL Server dostępne w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie v 11.0.
Sekcja serwera parametrów połączenia, których należy użyć, to "(LocalDB)\\v 11.0".
Należy pamiętać, aby użyć ciągu Verbatim poprzedzonego prefiksem `@` lub podwójnym ukośnikiem "\\\\" podczas określania parametrów połączenia w C# kodzie.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Wersja Entity Framework Tools dostępna w tej wersji programu Visual Studio nie jest zgodna ze środowiskiem uruchomieniowym Entity Framework 6 i nie może zostać uaktualniona.
- Domyślnie narzędzia Entity Framework spowodują dodanie Entity Framework 4,0 do projektów.
Aby można było tworzyć aplikacje przy użyciu dowolnych nowszych wersji EF, należy najpierw zainstalować [rozszerzenie Menedżera pakietów NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Domyślnie wszystkie generowanie kodu w wersji narzędzi EF bazują na obiektach EntityObject i Entity Framework 4.
Zaleca się przełączenie generowania kodu na podstawie DbContext i Entity Framework 5, instalując szablony generowania kodu kontekstu DbContext dla [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) lub [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Po zainstalowaniu rozszerzeń Menedżera pakietów NuGet można ręcznie zainstalować lub uaktualnić wszystkie pakiety NuGet EF dostępne online i używać EF6 z Code First, które nie wymagają projektanta.
- Domyślnie wystąpienie SQL Server dostępne w tej wersji programu Visual Studio jest SQL Server Express o nazwie SQLEXPRESS.
Sekcja serwera parametrów połączenia, które powinny być używane, to ".\\SQLEXPRESS ".
Należy pamiętać, aby użyć ciągu Verbatim poprzedzonego prefiksem `@` lub podwójnym ukośnikiem "\\\\" podczas określania parametrów połączenia w C# kodzie.
