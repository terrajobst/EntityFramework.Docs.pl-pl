---
title: Jak działają zapytania — EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136239"
---
# <a name="how-queries-work"></a>Jak działają kwerendy

Entity Framework Core używa języka zintegrowane zapytanie (LINQ) do kwerendy danych z bazy danych. LINQ umożliwia używanie języka C# (lub wybranego języka .NET) do pisania silnie wpisanych zapytań na podstawie kontekstu pochodnego i klas jednostek.

## <a name="the-life-of-a-query"></a>Okres ważności kwerendy

Poniżej przedstawiono omówienie wysokiego poziomu procesu, przez który przechodzi każda kwerenda.

1. Kwerenda LINQ jest przetwarzana przez entity framework core w celu utworzenia reprezentacji, która jest gotowa do przetworzenia przez dostawcę bazy danych
   1. Wynik jest buforowany, dzięki czemu przetwarzanie to nie musi być wykonywane za każdym razem, gdy kwerenda jest wykonywana
2. Wynik jest przekazywany do dostawcy bazy danych
   1. Dostawca bazy danych określa, które części kwerendy mogą być oceniane w bazie danych
   2. Te części kwerendy są tłumaczone na język kwerend specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych)
   3. Kwerenda jest wysyłana do bazy danych i zwracany zestaw wyników (wyniki są wartościami z bazy danych, a nie wystąpień encji)
3. Dla każdego elementu w zestawie wyników
   1. Jeśli jest to kwerenda śledząca, EF sprawdza, czy dane reprezentują jednostkę już w monitorze zmian dla wystąpienia kontekstu
      * Jeśli tak, istniejąca jednostka jest zwracana
      * Jeśli nie, tworzona jest nowa encja, jest skonfigurowane śledzenie zmian, a nowa encja jest zwracana
   2. Jeśli jest to kwerenda nie śledząca, nowa encja jest zawsze tworzona i zwracana

## <a name="when-queries-are-executed"></a>Podczas wykonywania zapytań

Podczas wywoływania operatorów LINQ, jesteś po prostu tworzenie reprezentacji w pamięci kwerendy. Kwerenda jest wysyłana do bazy danych tylko wtedy, gdy wyniki są używane.

Najczęstsze operacje, które powodują kwerendę wysyłane do bazy danych są:

* Iteracja wyników w `for` pętli
* Korzystanie z operatora, `ToArray` `Single`takiego `Count` jak `ToList`, , lub równoważne przeciążenia asynchroniczne

> [!WARNING]  
> **Zawsze sprawdzaj poprawność danych wejściowych użytkownika:** Podczas EF Core chroni przed atakami iniekcji SQL przy użyciu parametrów i uciekania literały w kwerendach, nie sprawdza poprawności danych wejściowych. Odpowiednie sprawdzanie poprawności, zgodnie z wymaganiami aplikacji, należy wykonać przed wartości z niezaufanych źródeł są używane w zapytaniach LINQ, przypisane do właściwości jednostki lub przekazywane do innych interfejsów API EF Core. Obejmuje to wszelkie dane wejściowe użytkownika używane do dynamicznego konstruowania zapytań. Nawet przy użyciu LINQ, jeśli akceptujesz dane wejściowe użytkownika do tworzenia wyrażeń, należy upewnić się, że tylko zamierzone wyrażenia mogą być konstruowane.
