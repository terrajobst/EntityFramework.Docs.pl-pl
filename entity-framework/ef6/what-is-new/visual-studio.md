---
title: Wersje programu Visual Studio — EF6
author: divega
ms.date: 2018-07-05
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: aa5409506eeea599421608ddb2dd891df0413961
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997388"
---
# <a name="visual-studio-releases"></a>Wersje programu Visual Studio

Firma Microsoft zaleca, aby zawsze używać najnowszej wersji programu Visual Studio, ponieważ zawiera najnowsze narzędzia dla platformy .NET, NuGet i Entity Framework.
W rzeczywistości różne przykłady i wskazówki dotyczące różnych dokumentację programu Entity Framework przyjęto założenie, że używasz najnowszej wersji programu Visual Studio.

Możliwe jest jednak używać starszych wersji programu Visual Studio z różnymi wersjami programu Entity Framework tak długo, jak długo potrwać do konta pewne różnice:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 w wersji 15.7 lub nowszej

- Ta wersja programu Visual Studio zawiera najnowszą wersję narzędzia Entity Framework i środowiskiem uruchomieniowym EF 6.2 i nie jest wymagane dodatkowe ustawienia.
Zobacz [What's New](~/ef6/what-is-new/index.md) Aby uzyskać więcej informacji o tych wersjach.
- Dodawanie programu Entity Framework do nowych projektów przy użyciu narzędzi programu EF automatycznie doda pakietu EF 6.2 NuGet.
Można ręcznie zainstalować lub uaktualnić do dowolnego pakietu NuGet programu EF dostępne online.
- Domyślnie wystąpienie programu SQL Server dostępnych w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie MSSQLLocalDB.
Sekcja serwera parametrów połączenia, należy użyć "(localdb)\\MSSQLLocalDB".
Pamiętaj, aby używać ciąg verbatim prefiksem `@` lub podwójny ukośnik odwrotny "\\\\" podczas określania parametrów połączenia w kodzie języka C#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015, Visual Studio 2017 w wersji 15.6

- Te wersje programu Visual Studio obejmują narzędzia Entity Framework i środowisko uruchomieniowe 6.1.3.
Zobacz [wydania w ciągu ostatnich](~/ef6/what-is-new/past-releases.md#ef-613) Aby uzyskać więcej informacji o tych wersjach.
- Dodawanie programu Entity Framework do nowych projektów przy użyciu narzędzi programu EF automatycznie doda EF 6.1.3 pakietu NuGet.
Można ręcznie zainstalować lub uaktualnić do dowolnego pakietu NuGet programu EF dostępne online.
- Domyślnie wystąpienie programu SQL Server dostępnych w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie MSSQLLocalDB.
Sekcja serwera parametrów połączenia, należy użyć "(localdb)\\MSSQLLocalDB".
Pamiętaj, aby używać ciąg verbatim prefiksem `@` lub podwójny ukośnik odwrotny "\\\\" podczas określania parametrów połączenia w kodzie języka C#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Ta wersja programu Visual Studio zawiera i starszą wersję narzędzi Entity Framework i środowiska uruchomieniowego.
Zaleca się uaktualnienie do programu Entity Framework Tools 6.1.3, za pomocą [Instalator](https://www.microsoft.com/en-us/download/details.aspx?id=40762) dostępne w programie Microsoft Download Center.
Zobacz [wydania w ciągu ostatnich](~/ef6/what-is-new/past-releases.md#ef-613) Aby uzyskać więcej informacji o tych wersjach.
- Dodawanie programu Entity Framework do nowych projektów przy użyciu uaktualnionego narzędzia EF automatycznie doda EF 6.1.3 pakietu NuGet.
Można ręcznie zainstalować lub uaktualnić do dowolnego pakietu NuGet programu EF dostępne online.
- Domyślnie wystąpienie programu SQL Server dostępnych w tej wersji programu Visual Studio jest wystąpieniem LocalDB o nazwie MSSQLLocalDB.
Sekcja serwera parametrów połączenia, należy użyć "(localdb)\\MSSQLLocalDB".
Pamiętaj, aby używać ciąg verbatim prefiksem `@` lub podwójny ukośnik odwrotny "\\\\" podczas określania parametrów połączenia w kodzie języka C#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Ta wersja programu Visual Studio zawiera i starszą wersję narzędzi Entity Framework i środowiska uruchomieniowego.
Zaleca się uaktualnienie do programu Entity Framework Tools 6.1.3, za pomocą [Instalator](https://www.microsoft.com/en-us/download/details.aspx?id=40762) dostępne w programie Microsoft Download Center.
Zobacz [wydania w ciągu ostatnich](~/ef6/what-is-new/past-releases.md#ef-613) Aby uzyskać więcej informacji o tych wersjach.
- Dodawanie programu Entity Framework do nowych projektów przy użyciu uaktualnionego narzędzia EF automatycznie doda EF 6.1.3 pakietu NuGet.
Można ręcznie zainstalować lub uaktualnić do dowolnego pakietu NuGet programu EF dostępne online.
- Domyślnie wystąpienie programu SQL Server dostępnych w tej wersji programu Visual Studio jest wystąpienia LocalDB, wywoływana w wersji 11.0.
Sekcja serwera parametrów połączenia, należy użyć "(localdb)\\w wersji 11.0".
Pamiętaj, aby używać ciąg verbatim prefiksem `@` lub podwójny ukośnik odwrotny "\\\\" podczas określania parametrów połączenia w kodzie języka C#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Wersja narzędzi Entity Framework Tools dostępnych w tej wersji programu Visual Studio nie jest zgodny ze środowiskiem uruchomieniowym platformy Entity Framework 6 i nie może zostać uaktualniona.
- Domyślnie narzędzia Entity Framework doda Entity Framework 4.0 do swoich projektów.
Aby tworzyć aplikacje przy użyciu dowolnej nowszych wersji EF, najpierw musisz zainstalować [rozszerzenia Menedżera pakietów NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- Domyślnie wszystkie generowanie kodu w wersji EF narzędzia opiera się na EntityObject i Entity Framework 4.
Firma Microsoft zaleca, przełącz się generowanie kodu była oparta na DbContext i Entity Framework 5, instalując DbContext szablonów generowania kodu dla [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) lub [języka Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Po zainstalowaniu rozszerzenia Menedżera pakietów NuGet można ręcznie zainstalować lub uaktualnić do dowolnego pakietu NuGet programu EF dostępne online i za pomocą platformy EF6 Code First platformy, które nie wymagają projektanta.
- Domyślnie wystąpienie programu SQL Server dostępnych w tej wersji programu Visual Studio jest program SQL Server Express, o nazwie SQLEXPRESS.
Sekcja serwera parametrów połączenia, należy użyć ". \\SQLEXPRESS ".
Pamiętaj, aby używać ciąg verbatim prefiksem `@` lub podwójny ukośnik odwrotny "\\\\" podczas określania parametrów połączenia w kodzie języka C#.
