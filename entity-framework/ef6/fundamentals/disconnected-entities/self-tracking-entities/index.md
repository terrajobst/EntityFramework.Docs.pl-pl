---
title: Samodzielnie śledzenia jednostek - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
caps.latest.revision: 3
ms.openlocfilehash: 2fb4e9f4d4008c57e90c49a011bebb320eb2bb58
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913528"
---
# <a name="self-tracking-entities"></a>Samodzielnie śledzenia jednostek

> [!IMPORTANT]
> Nie zaleca się przy użyciu szablonu samoobsługowego tracking jednostek. Tylko będą dostępne do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z wykresami odłączonych jednostek, należy wziąć pod uwagę inne alternatywy dla takich jak [słupkowych jednostek](http://trackableentities.github.io/), która jest podobna do samoobsługowego-Tracking-jednostek, które jest bardziej aktywnie rozwijany przez technologię Społeczność lub pisanie kodu niestandardowego za pomocą śledzenia interfejsów API zmian niskiego poziomu.

W aplikacji Entity Framework, na podstawie kontekstu jest odpowiedzialny za śledzenie zmian w obiekty. Możesz następnie użyć metody SaveChanges aby utrwalić zmiany w bazie danych. Podczas pracy z aplikacjami N-warstwowej, obiekty jednostki zwykle jest odłączony od kontekstu i należy zdecydować, jak śledzić zmiany i raportu te zmiany do kontekstu. Samodzielnie śledzenia jednostek (ste) może pomóc śledzić zmiany w dowolnej warstwy, a następnie odtworzenia te zmiany do kontekstu do zapisania.  

Użyj ste tylko wtedy, gdy kontekst nie jest dostępna w ramach warstwy gdzie zostały wprowadzone zmiany do obiektu wykresu. Jeśli kontekst jest dostępny, nie ma potrzeby używania ste, ponieważ zajmie się kontekst śledzenia zmian.  

Ten element szablon generuje dwa .TT — pliki (szablon tekstowy):  

- ** \<Nazwę modelu\>.tt** generuje plik typów jednostek i klasa pomocnika, która zawiera logikę śledzenia zmian, która jest używana przez własny śledzenia jednostek i metody rozszerzenia, które umożliwiają ustawianie stanu na własnym śledzenie jednostki.  
- ** \<Nazwę modelu\>. Context.TT** generuje plik pochodnej kontekstu i klasa rozszerzenia, która zawiera **applychanges —** metody **ObjectContext** i **obiektu ObjectSet** klasy. Te metody zbadania informacji śledzenia zmian, które znajduje się na wykresie własnym śledzenia jednostek wywnioskowania zestaw operacji, które należy wykonać, aby zapisać zmiany w bazie danych.  

## <a name="get-started"></a>Rozpocznij  

Aby rozpocząć pracę, odwiedź stronę [Self-Tracking jednostek wskazówki](walkthrough.md) strony.  

## <a name="considerations-when-working-with-self-tracking-entities"></a>Zagadnienia dotyczące pracy z własnym śledzenia jednostek  
> [!IMPORTANT]
> Nie zaleca się przy użyciu szablonu samoobsługowego tracking jednostek. Tylko będą dostępne do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z wykresami odłączonych jednostek, należy wziąć pod uwagę inne alternatywy dla takich jak [słupkowych jednostek](http://trackableentities.github.io/), która jest podobna do samoobsługowego-Tracking-jednostek, które jest bardziej aktywnie rozwijany przez technologię Społeczność lub pisanie kodu niestandardowego za pomocą śledzenia interfejsów API zmian niskiego poziomu.

Podczas pracy z własnym śledzenie jednostek, należy wziąć pod uwagę następujące czynności:  

- Upewnij się, że projekt klienta ma odwołanie do zestawu zawierającego typów jednostek. Jeśli dodasz tylko odwołanie do usługi do projektu klienta, projekt klienta użyje typy serwerów proxy usługi WCF i nie rzeczywiste własnym śledzenie typów jednostek. Oznacza to, że nie będzie można uzyskać funkcji automatyczne powiadomienie o zarządzających śledzenia jednostki na kliencie. Jeśli nie chcesz jej celowo i obejmuje dodatkowe typy jednostek, należy ręcznie ustawić informacji śledzenia zmian na klienta, aby zmiany wysyłane z powrotem do usługi.  
- Wywołania operacji usługi powinny być bezstanowe i Utwórz nowe wystąpienie obiektu kontekstu. Zalecamy również utworzenie obiektu kontekstu w **przy użyciu** bloku.  
- Kiedy wykres, który został zmodyfikowany na kliencie z usługą i wysyła następnie zamierzasz kontynuować pracę z tym samym wykresie na komputerze klienckim, należy ręcznie wykonać iterację wykresu i wywołania **AcceptChanges** metody dla każdego obiektu do Zresetuj śledzenie zmian.  

    > Jeśli obiekty w grafie zawierają właściwości wartościami wygenerowanych w bazie danych (na przykład wartości tożsamości lub współbieżności), platformy Entity Framework spowoduje zastąpienie wartości tych właściwości z wartościami bazy danych, wygenerowane po **SaveChanges** metoda jest wywoływana. Możesz zaimplementować operację usługi do zwrócenia zapisanych obiektów lub Podaj listę wartości wygenerowanej właściwości dla obiektów do klienta. Klient musiałby Zamień wystąpień obiektów lub wartości właściwości obiektu do obiektów lub wartości właściwości zwrócony przez operację usługi.  
- Scalanie wykresów z wielu żądań usługi może stanowić obiekty ze zduplikowanymi wartościami klucza w wynikowym wykresie. Entity Framework nie powoduje usunięcia obiektów z zduplikowane klucze po wywołaniu **applychanges —** metody, ale zamiast tego zgłasza wyjątek. Aby uniknąć wykresy ze zduplikowanymi wartościami klucza, postępuj zgodnie z jednym z wzorców opisanego w następujący wpis w blogu: [jednostek Self-Tracking: applychanges — i zduplikowanych podmiotów](http://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409).  
- Po zmianie relacji między obiektami, ustawiając właściwość klucza obcego referencyjna właściwość nawigacji jest ustawiona na wartość null i nie są zsynchronizowane do odpowiedniej jednostki podmiotu zabezpieczeń na komputerze klienckim. Po dołączeniu do kontekstu obiektów programu graph (na przykład, po wywołaniu metody **applychanges —** metoda), właściwości klucza obcego i właściwości nawigacji są synchronizowane.  

    > Nie ma właściwości nawigacji odwołania synchronizowane z odpowiedniego obiektu podmiotu zabezpieczeń może być problem, jeśli określono usuwanie kaskadowe w relacji klucza obcego. Jeśli usuniesz główny, Usuń nie będą przekazywane do obiektów zależnych. Jeśli masz kaskadowo określony, należy użyć właściwości nawigacji, aby zmienić relacji zamiast ustawiać właściwości klucza obcego.  
- Samodzielnie śledzenie jednostek nie są włączone do wykonywania ładowania z opóźnieniem.  
- Serializacja binarna i serializacji obiektów zarządzania stan programu ASP.NET nie jest obsługiwane przez własny śledzenie jednostek. Można jednak dostosować szablon, aby dodać obsługę serializacji binarnej. Aby uzyskać więcej informacji, zobacz [za pomocą serializacji binarnej i ViewState z jednostkami Self-Tracking](http://go.microsoft.com/fwlink/?LinkId=199208).  

### <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń  

Następujące zagadnienia dotyczące zabezpieczeń powinny brane pod uwagę podczas pracy z własnym śledzenie jednostki:  

- Usługa nie należy ufać żądań kierowanych do pobrania lub aktualizowanie danych w kliencie niezaufanej lub za pośrednictwem niezaufanej kanału. Klient musi zostać uwierzytelniony: bezpieczne koperty kanału lub komunikat powinien być używany. Aby upewnić się, że są one zgodne z oczekiwaniami i jest uzasadnione zmiany dla danego scenariusza, można sprawdzić poprawności żądania klientów do aktualizacji lub odbierać dane.  
- Należy unikać poufnych informacji jako klucze jednostek (na przykład numery ubezpieczenia społecznego). Zmniejsza to możliwość przypadkowo szeregowania informacje poufne na własnym śledzenie wykresy jednostki do klienta, który nie jest w pełni zaufany. Za pomocą skojarzeń niezależnie od oryginalnego klucza podmiotu, który jest powiązany z tą, która jest deserializowana mogą zostać wysłane do klienta, jak również.  
- Aby uniknąć propagowanie komunikaty o wyjątkach, zawierających poufne dane w warstwie klienta wywołania **applychanges —** i **SaveChanges** na serwerze warstwy musi być ujęte w kodzie obsługi wyjątków.  
