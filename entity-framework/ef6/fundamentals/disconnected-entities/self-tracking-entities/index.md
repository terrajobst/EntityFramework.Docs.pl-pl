---
title: Jednostki samoobsługowe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5e60f5be-7bbb-4bf8-835e-0ac808d6c84a
ms.openlocfilehash: 3bb9759d89fbd0c10b911625aa7d0afd7747de14
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419524"
---
# <a name="self-tracking-entities"></a>Jednostki samoobsługowe

> [!IMPORTANT]
> Nie zaleca się już używania szablonu jednostek samoobsywu. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonych wykresów jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [jednostki śledzone](https://trackableentities.github.io/), która jest technologia podobna do self-tracking-jednostek, który jest bardziej aktywnie opracowany przez społeczność lub pisania kodu niestandardowego przy użyciu interfejsów API śledzenia zmian niskiego poziomu.

W aplikacji opartej na platformie entity framework kontekst jest odpowiedzialny za śledzenie zmian w obiektach. Następnie należy użyć SaveChanges metody, aby utrwalić zmiany w bazie danych. Podczas pracy z aplikacjami N-warstwy obiekty jednostki są zwykle odłączone od kontekstu i należy zdecydować, jak śledzić zmiany i zgłaszać te zmiany z powrotem do kontekstu. Jednostki samoobsługowe (STE) mogą pomóc w śledzeniu zmian w dowolnej warstwie, a następnie odtwarzaniu tych zmian w kontekście, który ma zostać zapisany.  

Warstwy STEs należy używać tylko wtedy, gdy kontekst nie jest dostępny w warstwie, w której wprowadzane są zmiany w wykresie obiektów. Jeśli kontekst jest dostępny, nie ma potrzeby używania obiektów STEs, ponieważ kontekst zajmie się śledzeniem zmian.  

Ten element szablonu generuje dwa pliki .tt (szablon tekstu):  

- Nazwa ** \<modelu\>.tt** plik generuje typy jednostek i klasy pomocnika, który zawiera logikę śledzenia zmian, który jest używany przez jednostki samoobejmowania i metody rozszerzenia, które umożliwiają ustawienie stanu na jednostek samośledzenia.  
- ** \<Nazwa\>modelu . Context.tt** plik generuje kontekst pochodny i klasę rozszerzenia, która zawiera **metody ApplyChanges** dla **objectContext** i **ObjectSet** klas. Metody te badają informacje śledzenia zmian, które są zawarte na wykresie jednostek samośledzących, aby wywnioskować zestaw operacji, które muszą zostać wykonane w celu zapisania zmian w bazie danych.  

## <a name="get-started"></a>Rozpoczęcie pracy  

Aby rozpocząć, odwiedź [stronę Instruktaż jednostek samośledzenia.](walkthrough.md)  

## <a name="functional-considerations-when-working-with-self-tracking-entities"></a>Uwagi funkcjonalne podczas pracy z jednostkami samoobjudy  
> [!IMPORTANT]
> Nie zaleca się już używania szablonu jednostek samoobsywu. Będzie ona nadal dostępna tylko do obsługi istniejących aplikacji. Jeśli aplikacja wymaga pracy z odłączonych wykresów jednostek, należy wziąć pod uwagę inne alternatywy, takie jak [jednostki śledzone](https://trackableentities.github.io/), która jest technologia podobna do self-tracking-jednostek, który jest bardziej aktywnie opracowany przez społeczność lub pisania kodu niestandardowego przy użyciu interfejsów API śledzenia zmian niskiego poziomu.

Podczas pracy z jednostkami samoobejechowywych należy wziąć pod uwagę następujące kwestie:  

- Upewnij się, że projekt klienta ma odwołanie do zestawu zawierającego typy jednostek. Jeśli dodasz tylko odwołanie usługi do projektu klienta, projekt klienta użyje typów proxy WCF, a nie rzeczywistych typów jednostek samośledzących. Oznacza to, że nie otrzymasz automatycznych funkcji powiadomień, które zarządzają śledzeniem jednostek na kliencie. Jeśli celowo nie chcesz dołączyć typy jednostek, trzeba będzie ręcznie ustawić informacje śledzenia zmian na kliencie dla zmian, które mają być wysyłane z powrotem do usługi.  
- Wywołania operacji usługi powinny być bezstanowe i utworzyć nowe wystąpienie kontekstu obiektu. Zaleca się również utworzenie kontekstu obiektu w **using** bloku.  
- Podczas wysyłania wykresu, który został zmodyfikowany na kliencie do usługi, a następnie zamierzają kontynuować pracę z tym samym wykresem na kliencie, należy ręcznie iterować za pośrednictwem wykresu i **wywołać AcceptChanges** metody na każdy obiekt, aby zresetować śledzenia zmian.  

    > Jeśli obiekty na wykresie zawierają właściwości z wartościami generowanymi przez bazę danych (na przykład wartości tożsamości lub współbieżności), entity framework zastąpi wartości tych właściwości wartości generowanych przez bazę danych po **savechanges** metoda jest wywoływana. Można zaimplementować operację usługi, aby zwrócić zapisane obiekty lub listę wygenerowanych wartości właściwości dla obiektów z powrotem do klienta. Klient musiałby następnie zastąpić wystąpienia obiektu lub wartości właściwości obiektu wartościami obiektów lub właściwości zwróconych z operacji usługi.  
- Scalanie wykresów z wielu żądań serwisowych może wprowadzać obiekty z duplikatami wartości kluczy na wykresie wynikowym. Entity Framework nie usuwa obiektów z duplikatami kluczy podczas **wywoływania ApplyChanges** metody, ale zamiast tego zgłasza wyjątek. Aby uniknąć wykresów z zduplikowanymi wartościami kluczy, należy wykonać jeden z wzorców opisanych w następującym blogu: [Jednostki samośledzące: ApplyChanges i zduplikowane jednostki](https://go.microsoft.com/fwlink/?LinkID=205119&clcid=0x409).  
- Po zmianie relacji między obiektami przez ustawienie właściwości klucza obcego, właściwość nawigacji odwołania jest ustawiona na wartość null i nie jest synchronizowana z odpowiednią jednostką głównej na kliencie. Po wykres jest dołączony do kontekstu obiektu (na przykład po **wywołaniu ApplyChanges** metody), właściwości klucza obcego i właściwości nawigacji są synchronizowane.  

    > Brak właściwości nawigacji odwołania zsynchronizowany z odpowiednim obiektem głównym może być problem, jeśli określono kaskadowe usuwanie relacji klucza obcego. Jeśli usuniesz podmiot zabezpieczeń, delete nie będzie propagowany do obiektów zależnych. Jeśli określono kaskadowe usunięcia, użyj właściwości nawigacji, aby zmienić relacje zamiast ustawiać właściwość klucza obcego.  
- Jednostki samoobsługowe nie są włączone do wykonywania z opóźnieniem ładowania.  
- Binarna serializacja i serializacja do ASP.NET obiektów zarządzania stanem nie jest obsługiwana przez jednostki samośledzące. Można jednak dostosować szablon, aby dodać obsługę serializacji binarnej. Aby uzyskać więcej informacji, zobacz [Korzystanie z serializacji binarnej i viewstate z jednostkami samoobszukacyjnymi](https://go.microsoft.com/fwlink/?LinkId=199208).  

## <a name="security-considerations"></a>Zagadnienia związane z zabezpieczeniami  

Podczas pracy z jednostkami samośledzącymi należy wziąć pod uwagę następujące kwestie bezpieczeństwa:  

- Usługa nie powinna ufać żądaniom pobierania lub aktualizowania danych z niezaufanego klienta lub kanału niezaufanego. Klient musi być uwierzytelniony: należy użyć bezpiecznego kanału lub koperty wiadomości. Żądania klientów do aktualizacji lub pobierania danych muszą zostać zweryfikowane, aby upewnić się, że są one zgodne z oczekiwanymi i uzasadnionymi zmianami dla danego scenariusza.  
- Unikaj używania poufnych informacji jako kluczy encji (na przykład numerów ubezpieczenia społecznego). Zmniejsza to możliwość nieumyślnego serializacji poufnych informacji na wykresach jednostki samośledzącej do klienta, który nie jest w pełni zaufany. W przypadku niezależnych skojarzeń oryginalny klucz jednostki powiązanej z jednostką, która jest serializowana, może być również wysyłany do klienta.  
- Aby uniknąć propagowania komunikatów wyjątków, które zawierają poufne dane do warstwy klienta, **wywołania ApplyChanges** i **SaveChanges** w warstwie serwera powinny być zawinięte w kod obsługi wyjątków.  
