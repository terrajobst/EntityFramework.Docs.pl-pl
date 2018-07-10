---
title: Praca z jednostkami odłączonego - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 12138003-a373-4817-b1b7-724130202f5f
caps.latest.revision: 3
ms.openlocfilehash: 5419215a77b57ab3c92fb88a512510070ea23bd6
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913447"
---
# <a name="working-with-disconnected-entities"></a>Praca z odłączone jednostki
W aplikacji Entity Framework, na podstawie klasy kontekstu jest odpowiedzialny za wykrywanie zmian zastosowanych do śledzonych jednostek. Wywołanie metody SaveChanges utrzymuje zmian śledzonych przez kontekst do bazy danych. Podczas pracy z aplikacjami n warstwowej, obiekty jednostki są zazwyczaj modyfikowane przy braku połączenia z kontekstu i należy zdecydować, jak śledzić zmiany i raportu te zmiany do kontekstu. W tym temacie omówiono różne opcje, które są dostępne, gdy używający narzędzia Entity Framework o odłączony jednostek.   

## <a name="web-service-frameworks"></a>Struktury usługi sieci Web

Technologie usług sieci Web zwykle obsługują wzorce, które mogą służyć do utrwalenia zmian na poszczególne obiekty odłączonych. Na przykład ASP.NET Web API umożliwia akcji kontrolera kodu, które może obejmować wywołania do EF, aby zachować zmiany wprowadzone do obiektu w bazie danych. W rzeczywistości internetowy interfejs API, narzędzi w programie Visual Studio ułatwia tworzenia szkieletu kontrolera interfejsu API sieci Web z modelu Entity Framework 6. Aby uzyskać więcej informacji, zobacz [przy użyciu interfejsu API sieci Web za pomocą platformy Entity Framework 6](https://docs.microsoft.com/en-us/aspnet/web-api/overview/data/using-web-api-with-entity-framework/).   

W przeszłości były kilka innych technologii sieci Web usługi, które oferowane integracji z programem Entity Framework, takie jak [usług danych WCF](https://docs.microsoft.com/dotnet/framework/data/wcf/create-a-data-service-using-an-adonet-ef-data-wcf) i [RIA Services](https://docs.microsoft.com/en-us/previous-versions/dotnet/wcf-ria/ee707344(v=vs.91)).

## <a name="low-level-ef-apis"></a>Interfejsy API niskiego poziomu EF

Jeśli nie chcesz użyć istniejącego rozwiązania n warstwowa lub jeśli chcesz dostosować, co się dzieje w akcji kontrolera, w usługach interfejsu API sieci Web, platformy Entity Framework udostępnia interfejsy API, które umożliwiają zastosowanie zmian wprowadzonych w warstwie odłączonych. Aby uzyskać więcej informacji, zobacz [stanu Dodaj, Dołącz i jednostki](~/ef6/saving/change-tracking/entity-state.md).  

## <a name="self-tracking-entities"></a>Samodzielnie śledzenie jednostek  

Śledzenie zmian na wykresach dowolnego jednostek przy braku połączenia z kontekstu EF jest twardych problem. Jeden z próbuje rozwiązać problem był szablonu generacji kodu Self-Tracking jednostek. Ten szablon generuje klasy jednostek, które zawierają logikę do śledzenia zmian wprowadzanych w warstwie odłączonego jako stan w samych jednostkach. Zestaw metod rozszerzenia zostanie również wygenerowany tak, aby zastosować te zmiany do kontekstu.

Ten szablon może być używany z modeli utworzonych za pomocą projektanta EF, ale nie można używać w modelach Code First. Aby uzyskać więcej informacji, zobacz [jednostek Self-Tracking](self-tracking-entities/index.md).  

> [!IMPORTANT]
> Nie zaleca się przy użyciu szablonu samoobsługowego tracking jednostek. Tylko będą dostępne do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z wykresami odłączonych jednostek, należy wziąć pod uwagę inne alternatywy dla takich jak [słupkowych jednostek](http://trackableentities.github.io/), która jest podobna do samoobsługowego-Tracking-jednostek, które jest bardziej aktywnie rozwijany przez technologię Społeczność lub pisanie kodu niestandardowego za pomocą śledzenia interfejsów API zmian niskiego poziomu.
