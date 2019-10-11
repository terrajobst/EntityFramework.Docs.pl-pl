---
title: Praca z odłączonymi jednostkami — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
ms.openlocfilehash: f1ce44e7b00ec4c60a81ed850ce5c9d866495e1b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181767"
---
# <a name="working-with-disconnected-entities"></a>Praca z odłączonymi jednostkami
W aplikacji opartej na Entity Framework Klasa kontekstowa jest odpowiedzialna za wykrywanie zmian zastosowanych do śledzonych jednostek. Wywołanie metody metody SaveChanges zachowuje zmiany śledzone przez kontekst do bazy danych. Podczas pracy z aplikacjami n-warstwowymi obiekty jednostek są zwykle modyfikowane podczas odłączania od kontekstu i należy zdecydować, jak śledzić zmiany i raportować te zmiany z powrotem do kontekstu. W tym temacie omówiono różne opcje, które są dostępne w przypadku korzystania z Entity Framework z odłączonymi jednostkami.   

## <a name="web-service-frameworks"></a>Struktury usług sieci Web

Technologie usług sieci Web zwykle obsługują wzorce, które mogą być używane do utrwalania zmian w pojedynczych odłączonych obiektach. Na przykład interfejs API sieci Web ASP.NET umożliwia kod akcji kontrolera, które mogą obejmować wywołania EF, aby zachować zmiany wprowadzone do obiektu w bazie danych. W rzeczywistości narzędzia internetowego interfejsu API w programie Visual Studio ułatwiają tworzenie szkieletu kontrolera interfejsu API sieci Web z modelu Entity Framework 6. Aby uzyskać więcej informacji, zobacz [Korzystanie z interfejsu API sieci Web w Entity Framework 6](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

W przeszłości istniały kilka innych technologii usług sieci Web, które oferują integrację z Entity Framework, takie jak [usługi danych programu WCF](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) i [usługi RIA](https://docs.microsoft.com/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>Interfejsy API EF niskiego poziomu

Jeśli nie chcesz korzystać z istniejącego rozwiązania n-warstwowego lub chcesz dostosować działanie w ramach akcji kontrolera w usługach interfejsu API sieci Web, Entity Framework udostępnia interfejsy API, które umożliwiają zastosowanie zmian wprowadzonych w odłączonej warstwie. Aby uzyskać więcej informacji, zobacz [Dodawanie, dołączanie i stan jednostki](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Jednostki samośledzenia  

Śledzenie zmian na dowolnych wykresach jednostek niepołączonych z kontekstem EF jest problemem. Jedną z prób rozwiązania tego problemu jest szablon generowania kodu samośledzących jednostek. Ten szablon generuje klasy jednostek, które zawierają logikę do śledzenia zmian wprowadzonych w odłączonej warstwie jako stan w samych jednostkach. Zestaw metod rozszerzających jest również generowany, aby zastosować te zmiany do kontekstu.

Tego szablonu można używać z modelami utworzonymi przy użyciu programu EF Designer, ale nie można ich używać z modelami Code First. Aby uzyskać więcej informacji, zobacz [samośledzące jednostki](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Nie zalecamy już korzystania z szablonu samośledzenia jednostek. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonymi wykresami jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [śledzone jednostki](https://trackableentities.github.io/), które są technologią podobną do samodzielnego śledzenia jednostek, które są opracowywane przez społeczność lub piszą kod niestandardowy korzystający z interfejsów API śledzenia zmian niskiego poziomu.
