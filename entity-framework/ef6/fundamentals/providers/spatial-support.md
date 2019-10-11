---
title: Obsługa dostawcy dla typów przestrzennych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181597"
---
# <a name="provider-support-for-spatial-types"></a>Obsługa dostawcy dla typów przestrzennych
Entity Framework obsługuje pracę z danymi przestrzennymi za pomocą klas DbGeography lub DbGeometry. Te klasy są oparte na funkcjach specyficznych dla bazy danych oferowanych przez dostawcę Entity Framework. Nie wszyscy dostawcy obsługują dane przestrzenne, a te, które mogą mieć dodatkowe wymagania wstępne, takie jak instalacja zestawów typów przestrzennych. Więcej informacji o obsłudze dostawcy dla typów przestrzennych znajduje się poniżej.  

Dodatkowe informacje na temat korzystania z typów przestrzennych w aplikacji można znaleźć w dwóch przewodnikach, jeden dla Code First, drugi dla Database First lub Model First:  

- [Typy danych przestrzennych w Code First](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Typy danych przestrzennych w projektancie EF](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Wersje EF obsługujące typy przestrzenne  

Obsługa typów przestrzennych została wprowadzona w EF5. Jednak w przypadku typów przestrzennych EF5 są obsługiwane tylko wtedy, gdy aplikacja jest uruchamiana i działa na platformie .NET 4,5.  

Począwszy od typów przestrzennych EF6 są obsługiwane w przypadku aplikacji przeznaczonych dla programów .NET 4 i .NET 4,5.  

## <a name="ef-providers-that-support-spatial-types"></a>Dostawcy EF, którzy obsługują typy przestrzenne  

### <a name="ef5"></a>EF5  

Entity Framework dostawców dla EF5, że mamy świadomość, że typy przestrzenne są następujące:  

- Dostawca Microsoft SQL Server  
    - Ten dostawca jest dostarczany jako część EF5.  
    - Ten dostawca zależy od pewnych dodatkowych bibliotek niskiego poziomu, które mogą wymagać zainstalowania — Zobacz poniżej, aby uzyskać szczegółowe informacje.  
- [Devart dotConnect dla programu Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Jest to dostawca innych firm od Devart.  

Jeśli znasz dostawcę EF5, który obsługuje typy przestrzenne, skontaktuj się z nami i będziemy mogli dodać go do tej listy.  

### <a name="ef6"></a>EF6  

Entity Framework dostawców dla EF6, że mamy świadomość, że typy przestrzenne są następujące:  

- Dostawca Microsoft SQL Server  
    - Ten dostawca jest dostarczany jako część EF6.  
    - Ten dostawca zależy od pewnych dodatkowych bibliotek niskiego poziomu, które mogą wymagać zainstalowania — Zobacz poniżej, aby uzyskać szczegółowe informacje.  
- [Devart dotConnect dla programu Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Jest to dostawca innych firm od Devart.  

Jeśli znasz dostawcę EF6, który obsługuje typy przestrzenne, skontaktuj się z nami i będziemy mogli dodać go do tej listy.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Wymagania wstępne dotyczące typów przestrzennych z Microsoft SQL Server  

Obsługa SQL Server przestrzennej zależy od typów SqlGeography i SqlGeometry o niskim poziomie SQL Server. Te typy na żywo w zestawie Microsoft. SqlServer. Types. dll i ten zestaw nie jest dostarczany jako część EF lub jako część .NET Framework.  

Gdy program Visual Studio jest zainstalowany, często instaluje również wersję SQL Server i obejmuje instalację Microsoft. SqlServer. Types. dll.  

Jeśli SQL Server nie jest zainstalowana na komputerze, na którym mają być używane typy przestrzenne, lub jeśli typy przestrzenne zostały wykluczone z instalacji SQL Server, należy zainstalować je ręcznie. Typy można instalować przy użyciu `SQLSysClrTypes.msi`, który jest częścią Microsoft SQL Server Feature Pack. Typy przestrzenne są SQL Server specyficzne dla wersji, dlatego zalecamy [przeszukanie "SQL Server pakiet Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) w centrum pobierania Microsoft, a następnie wybranie i pobranie opcji odpowiadającej używanej wersji SQL Server.
