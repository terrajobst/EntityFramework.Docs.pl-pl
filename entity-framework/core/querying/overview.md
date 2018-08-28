---
title: Jak zapytania praca - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: f1c23471bfbc998b2d4f9dc579d1404d6202e109
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993206"
---
# <a name="how-queries-work"></a>Jak działają zapytań

Entity Framework Core używa Language Integrated Query (LINQ) do zapytania o dane z bazy danych. LINQ umożliwia przy użyciu języka C# (lub wybranym języku .NET) do pisania zapytań silnie typizowaną oparte na klasach pochodnych kontekstu i jednostki.

## <a name="the-life-of-a-query"></a>Czas życia zapytania

Poniżej znajduje się przegląd wysokiego poziomu procesu, który przechodzi każdego zapytania.

1. Zapytania LINQ są przetwarzane przez program Entity Framework Core do budowania reprezentacji, które jest gotowe do przetworzenia przez dostawcę bazy danych
   1. Wynik jest buforowany, dzięki czemu to przetwarzanie nie musi odbywać się za każdym razem, gdy zapytanie jest wykonywane
2. Wynik jest przekazywany do dostawcy bazy danych
   1. Dostawca bazy danych identyfikuje części zapytania, które mogą być obliczane w bazie danych
   2. Te części zapytania są tłumaczone na języka użytego zapytania bazy danych (na przykład SQL dla relacyjnej bazy danych)
   3. Co najmniej jeden zapytania są wysyłane do bazy danych i zwróconym zestawie wyników (wyniki są wartości z bazy danych, a nie wystąpień jednostek)
3. Dla każdego elementu w zestawie wyników
   1. Jeśli jest to zapytania śledzenia, EF sprawdza, czy dane reprezentuje jednostkę już w śledzenie zmian dla wystąpienia kontekstu
      * Jeśli tak, zwracany jest istniejąca jednostka
      * W przeciwnym razie jest tworzona nowa jednostka skonfigurowano śledzenie zmian i zwracany jest nowa jednostka
   2. Jeśli zapytania śledzenia nie EF sprawdza, czy dane reprezentuje jednostkę już w zestawu wyników dla tego zapytania
      * Jeśli tak, jest zwracana istniejącej jednostki <sup>(1)</sup>
      * Jeśli nie, nowy obiekt jest tworzony i zwracany

<sup>(1) </sup> Nie śledzenia zapytań za pomocą słabe odwołania do śledzenie jednostek, które już zostały zwrócone. Jeśli poprzedni wynik z tą samą tożsamością wykracza poza zakres, a następnie uruchamia wyrzucanie elementów bezużytecznych, może pojawić się nowe wystąpienie jednostki.

## <a name="when-queries-are-executed"></a>Gdy zapytania są wykonywane.

Po wywołaniu operatorów LINQ, są po prostu budowania reprezentacji w pamięci, zapytania. Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy wyniki są używane.

Najbardziej typowe operacje, których wynikiem zapytania są wysyłane do bazy danych są:
* Iteracja wyniki w parametrze `for` pętli
* Przy użyciu operatora, takich jak `ToList`, `ToArray`, `Single`, `Count`
* Powiązanie danych z wynikami zapytania do interfejsu użytkownika

> [!WARNING]  
> **Zawsze weryfikowały dane wejściowe użytkownika:** EF podczas zapewniania ochrony przed atakami polegającymi na iniekcji SQL, nie ma żadnych ogólnej walidacji danych wejściowych. W związku z tym jeśli wartości są przekazywane do interfejsów API, które są używane w kwerendach LINQ, przypisany do właściwości obiektu itd., pochodzi z niezaufanego źródła, a następnie odpowiednią sprawdzania poprawności, zgodnie z wymaganiami aplikacji powinny być wykonywane. Dotyczy to danych podawanych przez użytkownika używane do dynamicznego utworzenia kwerendy. Nawet wtedy, gdy za pomocą LINQ, jeśli akceptujesz, że dane wejściowe podane użytkownika, aby zbudować wyrażenia, potrzebne do upewnij się, niż tylko zamierzony wyrażenia można skonstruować.
