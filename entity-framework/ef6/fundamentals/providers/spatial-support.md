---
title: Obsługa dostawców dla typów przestrzennych - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022278"
---
# <a name="provider-support-for-spatial-types"></a>Obsługa dostawców dla typów przestrzennych
Entity Framework obsługuje pracę z danymi przestrzennymi za pośrednictwem DbGeography lub DbGeometry klasy. W ramach tych zajęć, zależą od funkcji specyficznych dla bazy danych udostępnianymi przez dostawcę programu Entity Framework. Nie wszyscy dostawcy obsługuje dane przestrzenne i tych, które wykonują może mieć dodatkowe wymagania wstępne, takich jak instalacja zestawów typów przestrzennych. Więcej informacji na temat Obsługa dostawców dla typów przestrzennych znajduje się poniżej.  

Dodatkowe informacje dotyczące sposobu używania typów przestrzennych w aplikacji znajdują się w dwóch wskazówki, jeden dla Code First, drugie dla pierwszej bazy danych lub pierwszego modelu:  

- [Typy w kodzie najpierw danych przestrzennych](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Typy danych przestrzennych w Projektancie platformy EF](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Wersje programu EF, które obsługują typów przestrzennych  

Obsługa typów przestrzennych została wprowadzona w EF5. Jednak w EF5 typów przestrzennych są obsługiwane tylko w przypadku aplikacji jest przeznaczony dla i działa w .NET 4.5.  

Począwszy od platformy EF6 typów przestrzennych są obsługiwane w przypadku aplikacji przeznaczonych dla platformy .NET 4.5 i .NET 4.  

## <a name="ef-providers-that-support-spatial-types"></a>EF dostawcy, obsługujące typów przestrzennych  

### <a name="ef5"></a>EF5  

Dostawcy programu Entity Framework do EF5, które sobie sprawę z czy typów przestrzennych pomocy technicznej:  

- Dostawca programu Microsoft SQL Server  
    - Ten dostawca jest dostarczany jako część EF5.  
    - Ten dostawca jest zależna od niektórych dodatkowych bibliotek niskiego poziomu, które muszą zostać zainstalowane — Zobacz szczegóły poniżej.  
- [Devart dotConnect na oprogramowanie Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Jest to dostawca innych firm z Devart.  

Jeśli znasz EF5 dostawcy obsługująca typów przestrzennych następnie można uzyskać w kontakcie i będziemy wszystkiego dodać go do tej listy.  

### <a name="ef6"></a>EF6  

Dostawcy programu Entity Framework, dla platformy EF6, które sobie sprawę z czy typów przestrzennych pomocy technicznej:  

- Dostawca programu Microsoft SQL Server  
    - Ten dostawca jest dostarczany jako część platformy EF6.  
    - Ten dostawca jest zależna od niektórych dodatkowych bibliotek niskiego poziomu, które muszą zostać zainstalowane — Zobacz szczegóły poniżej.  
- [Devart dotConnect na oprogramowanie Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Jest to dostawca innych firm z Devart.  

Jeśli znasz EF6 dostawcy obsługująca typów przestrzennych następnie można uzyskać w kontakcie i będziemy wszystkiego dodać go do tej listy.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Wymagania wstępne dotyczące typów przestrzennych z programem Microsoft SQL Server  

Obsługa przestrzenne programu SQL Server jest zależna od niskiego poziomu, specyficzne dla programu SQL Server typu SqlGeography i SqlGeometry. Te typy na żywo w zestawie Microsoft.SqlServer.Types.dll i ten zestaw nie jest dostarczany jako część EF lub w ramach programu .NET Framework.  

Po zainstalowaniu programu Visual Studio zostanie często również zainstalowana wersja programu SQL Server i będzie to obejmowało instalacji Microsoft.SqlServer.Types.dll.  

Jeśli program SQL Server nie jest zainstalowany na komputerze, na których chcesz użyć typów przestrzennych lub typów przestrzennych zostały wykluczone z instalacji programu SQL Server, należy je zainstalować ręcznie. Typy można zainstalować przy użyciu `SQLSysClrTypes.msi`, który jest częścią programu Microsoft SQL Server Feature Pack. Typów przestrzennych programu SQL Server specyficzny dla wersji są, tak więc zaleca się [Wyszukaj "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) w programie Microsoft Download Center, a następnie wybierz i Pobierz opcji, która odnosi się do wersji programu SQL Server, które będą używane.
