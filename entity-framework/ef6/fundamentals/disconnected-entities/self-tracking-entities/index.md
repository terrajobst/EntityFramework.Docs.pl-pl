---
title: Jednostki samośledzenia — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181727"
---
# <a name="self-tracking-entities"></a>Jednostki samośledzenia

> [!IMPORTANT]
> Nie zalecamy już korzystania z szablonu samośledzenia jednostek. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonymi wykresami jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [śledzone jednostki](https://trackableentities.github.io/), które są technologią podobną do samodzielnego śledzenia jednostek, które są opracowywane przez społeczność lub piszą kod niestandardowy korzystający z interfejsów API śledzenia zmian niskiego poziomu.

W aplikacji opartej na Entity Framework kontekście jest odpowiedzialny za śledzenie zmian w obiektach. Następnie należy użyć metody metody SaveChanges, aby zachować zmiany w bazie danych. W przypadku korzystania z aplikacji N-warstwowych obiekty jednostek są zazwyczaj odłączane od kontekstu i należy zdecydować, jak śledzić zmiany i raportować te zmiany z powrotem do kontekstu. Jednostki samodzielnego śledzenia (inżynierów Ste) mogą pomóc w śledzeniu zmian w dowolnej warstwie, a następnie powtórzeniu tych zmian w kontekście, który ma zostać zapisany.  

Używaj inżynierów Ste tylko wtedy, gdy kontekst nie jest dostępny w warstwie, w której są wykonywane zmiany grafu obiektów. Jeśli kontekst jest dostępny, nie ma potrzeby używania inżynierów Ste, ponieważ kontekst zajmie się śledzeniem zmian.  

Ten element szablonu generuje dwa pliki TT (szablon tekstowy):  

- Plik **\<model Name\>.tt** generuje typy jednostek i klasę pomocnika, która zawiera logikę śledzenia zmian, która jest używana przez jednostki samośledzenia i metody rozszerzające, które zezwalają na Ustawianie stanu dla jednostek samodzielnego śledzenia.  
- **@No__t-1model nazwa @ no__t-2. Plik Context.tt** generuje kontekst pochodny i klasę rozszerzenia, która zawiera **metody ApplyChanges —** dla klas **ObjectContext** i **ObjectSet** . Te metody sprawdzają informacje o śledzeniu zmian, które są zawarte na grafie jednostek samodzielnego śledzenia, aby wywnioskować zestaw operacji, które należy wykonać w celu zapisania zmian w bazie danych.  

## <a name="get-started"></a>Rozpocznij  

Aby rozpocząć, odwiedź stronę [przewodnika Samośledzenia jednostek](walkthrough.md) .  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Zagadnienia funkcjonalne podczas pracy z jednostkami samośledzenia  
> [!IMPORTANT]
> Nie zalecamy już korzystania z szablonu samośledzenia jednostek. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonymi wykresami jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [śledzone jednostki](https://trackableentities.github.io/), które są technologią podobną do samodzielnego śledzenia jednostek, które są opracowywane przez społeczność lub piszą kod niestandardowy korzystający z interfejsów API śledzenia zmian niskiego poziomu.

Podczas pracy z jednostkami samośledzenia należy wziąć pod uwagę następujące kwestie:  

- Upewnij się, że projekt klienta zawiera odwołanie do zestawu zawierającego typy jednostek. W przypadku dodania do projektu klienta tylko odwołania do usługi, projekt klienta będzie używać typów serwera proxy WCF, a nie rzeczywistych typów jednostek samośledzenia. Oznacza to, że nie zostaną wyświetlone funkcje powiadomień automatycznych, które zarządzają śledzeniem jednostek na kliencie. Jeśli celowo nie chcesz uwzględniać typów jednostek, musisz ręcznie ustawić informacje o śledzeniu zmian na kliencie, aby zmiany zostały wysłane z powrotem do usługi.  
- Wywołania operacji usługi powinny być bezstanowe i utworzyć nowe wystąpienie kontekstu obiektu. Zalecamy również utworzenie kontekstu obiektu w bloku **using** .  
- Po wysłaniu grafu, który został zmodyfikowany na kliencie do usługi, a następnie chcesz kontynuować pracę z tym samym wykresem na kliencie, musisz ręcznie wykonać iterację w grafie i wywołać metodę **AcceptChanges** dla każdego obiektu, aby zresetować zmianę. śledzenia.  

    > Jeśli obiekty w grafie zawierają właściwości z wartościami generowanymi przez bazę danych (na przykład tożsamości lub wartości współbieżności), Entity Framework zastąpią wartości tych właściwości wartościami generowanymi przez bazę danych po metodzie **metody SaveChanges** wywołan. Możesz zaimplementować operację usługi w celu zwrócenia zapisywanych obiektów lub listy wygenerowanych wartości właściwości dla obiektów z powrotem do klienta. Następnie klient musi zamienić wystąpienia obiektu lub wartości właściwości obiektu na wartości obiektów lub właściwości zwróconych z operacji usługi.  
- Scalanie wykresów z wielu żądań obsługi może spowodować, że obiekty mające zduplikowane wartości klucza na wykresie wyników. Entity Framework nie usuwa obiektów ze zduplikowanymi kluczami podczas wywoływania metody **ApplyChanges —** , ale zamiast tego zgłasza wyjątek. Aby uniknąć występowania wykresów zawierających zduplikowane wartości klucza, należy przestrzegać jednego z wzorców opisanych w następującym blogu: Jednostki śledzenia @no__t 0Self: ApplyChanges — i zduplikowane jednostki @ no__t-0.  
- W przypadku zmiany relacji między obiektami przez ustawienie właściwości klucza obcego właściwość nawigacji odwołania ma wartość null i nie jest zsynchronizowana z odpowiednią jednostką główną na komputerze klienckim. Po dołączeniu grafu do kontekstu obiektu (na przykład po wywołaniu metody **ApplyChanges —** ) właściwości klucza obcego i właściwości nawigacji są synchronizowane.  

    > Nie ma właściwości nawigacji referencyjnej synchronizowanej z odpowiednim obiektem Principal może być problemem, jeśli określono kaskadowe usuwanie w relacji klucza obcego. Usunięcie podmiotu zabezpieczeń spowoduje, że usunięcie nie zostanie propagowane do obiektów zależnych. Jeśli określono kaskadowe usuwanie, użyj właściwości nawigacji, aby zmienić relacje zamiast ustawiania właściwości klucza obcego.  
- Jednostki samośledzące nie są włączone, aby wykonać ładowanie z opóźnieniem.  
- Serializacja binarna i Serializacja do obiektów ASP.NET State Management nie są obsługiwane przez jednostki samośledzenia. Można jednak dostosować szablon, aby dodać obsługę serializacji binarnej. Aby uzyskać więcej informacji, zobacz [Używanie serializacji binarnej i elementu ViewState z jednostkami samośledzenia](https://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń  

Podczas pracy z jednostkami samodzielnego śledzenia należy wziąć pod uwagę następujące zagadnienia dotyczące zabezpieczeń:  

- Usługa nie powinna ufać żądaniami pobierania lub aktualizowania danych z niezaufanego klienta lub niezaufanego kanału. Klient musi zostać uwierzytelniony: należy użyć bezpiecznego kanału lub koperty komunikatów. Żądania klientów dotyczące aktualizowania lub pobierania danych muszą zostać sprawdzone, aby upewnić się, że są one zgodne z oczekiwanymi i uzasadnionymi zmianami w danym scenariuszu.  
- Unikaj używania poufnych informacji jako kluczy jednostek (na przykład numerów ubezpieczenia społecznego). Pozwala to ograniczyć możliwość przypadkowego serializacji informacji poufnych na wykresach jednostek, które nie są w pełni zaufane. Z niezależnymi skojarzeniami oryginalny klucz jednostki powiązanej z tym, który jest serializowany, może być również wysyłany do klienta.  
- Aby uniknąć propagowania komunikatów o wyjątkach zawierających dane poufne do warstwy klienta, wywołania do **ApplyChanges —** i **metody SaveChanges** w warstwie serwera powinny być opakowane w kod obsługi wyjątków.  
