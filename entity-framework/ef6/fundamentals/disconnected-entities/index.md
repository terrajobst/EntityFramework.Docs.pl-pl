---
title: Praca z odłączonych encjami — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419527"
---
# <a name="working-with-disconnected-entities"></a>Praca z odłączonych encjami
W aplikacji opartej na platformie entity framework klasa kontekstu jest odpowiedzialna za wykrywanie zmian stosowanych do śledzonych jednostek. Wywołanie SaveChanges metoda utrzymuje zmiany śledzone przez kontekst do bazy danych. Podczas pracy z aplikacjami n-warstwowych obiekty encji są zwykle modyfikowane po odłączeniu od kontekstu i należy zdecydować, jak śledzić zmiany i zgłaszać te zmiany z powrotem do kontekstu. W tym temacie omówiono różne opcje, które są dostępne podczas korzystania z entity framework z rozłączonych jednostek.   

## <a name="web-service-frameworks"></a>Struktury usług sieci Web

Technologie usług sieci Web zazwyczaj obsługują wzorce, których można używać do utrwalania zmian w poszczególnych odłączonych obiektach. Na przykład ASP.NET interfejsu API sieci Web umożliwia kod akcji kontrolera, które mogą obejmować wywołania EF do utrwalania zmian wprowadzonych do obiektu w bazie danych. W rzeczywistości narzędzia interfejsu API sieci Web w programie Visual Studio ułatwia tworzenie szkieletu kontrolera interfejsu API sieci Web z modelu entity framework 6. Aby uzyskać więcej informacji, zobacz [korzystanie z interfejsu API sieci Web z entity framework 6](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

W przeszłości istniało kilka innych technologii usług sieci Web, które oferowały integrację z entity framework, takie jak [WCF Data Services](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) i [RIA Services.](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91))

## <a name="low-level-ef-apis"></a>Interfejsy API EF niskiego poziomu

Jeśli nie chcesz używać istniejącego rozwiązania n-warstwowego lub jeśli chcesz dostosować, co dzieje się wewnątrz akcji kontrolera w usługach interfejsu API sieci Web, entity framework udostępnia interfejsy API, które umożliwiają stosowanie zmian wprowadzonych w warstwie rozłączone. Aby uzyskać więcej informacji, zobacz [Dodawanie, dołączanie i stan encji](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Jednostki samoobsługowe  

Śledzenie zmian na dowolnych wykresach jednostek, gdy są odłączone od kontekstu EF jest trudnym problemem. Jedną z prób rozwiązania problemu był szablon generowania kodu self-tracking jednostek. Ten szablon generuje klasy jednostek, które zawierają logikę do śledzenia zmian wprowadzonych w warstwie rozłączone jako stan w samych jednostkach. Zestaw metod rozszerzenia jest również generowany w celu zastosowania tych zmian w kontekście.

Ten szablon może być używany z modelami utworzonymi przy użyciu projektanta EF, ale nie można go używać z modelami Code First. Aby uzyskać więcej informacji, zobacz [Jednostki samoobsługowe](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Nie zaleca się już używania szablonu jednostek samoobsywu. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonych wykresów jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [jednostki śledzone](https://trackableentities.github.io/), która jest technologia podobna do self-tracking-jednostek, który jest bardziej aktywnie opracowany przez społeczność lub pisania kodu niestandardowego przy użyciu interfejsów API śledzenia zmian niskiego poziomu.
