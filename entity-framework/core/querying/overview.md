---
title: Jak odpytuje Praca - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Jak działają zapytań

Entity Framework Core używa języka integracji zapytania (LINQ) wykonać zapytania o dane z bazy danych. LINQ umożliwia przy użyciu języka C# (lub z języka .NET) do zapisania jednoznacznie zapytań opartych na klas pochodnych kontekstu i jednostki.

## <a name="the-life-of-a-query"></a>Czas życia zapytania

Poniżej znajduje się wysokiego poziomu Omówienie procesu, który przechodzi każdego zapytania.

1. Zapytania LINQ jest przetwarzany przez Entity Framework Core do budowania reprezentacji, który jest gotowy do przetwarzania dostawca bazy danych
   1. Wynik jest buforowany, dzięki czemu to przetwarzanie nie musi odbywać się za każdym razem, gdy zapytanie jest wykonywane
2. Wynik jest przekazywany do dostawcy bazy danych
   1. Dostawca bazy danych identyfikuje części zapytania, które może przyjąć w bazie danych
   2. Te części zapytania są tłumaczone na język określonej kwerendy bazy danych (np. SQL relacyjnej bazy danych)
   3. Co najmniej jednego zapytania są wysyłane do bazy danych i zestaw zwrócony wyników (wyniki są wartości z bazy danych, nie wystąpień jednostek)
3. Dla każdego elementu w zestawie wyników
   1. Jeśli jest to zapytanie śledzenia, EF sprawdza, czy dane reprezentuje jednostkę już w śledzenia zmian dla wystąpienia kontekstu
      * Jeśli tak, zwracany jest istniejącej jednostki
      * Jeśli nie jest tworzony nowy obiekt, skonfigurowano śledzenie zmian i nowy obiekt jest zwracany
   2. Jeśli jest to zapytanie nie śledzenia, EF sprawdza, czy dane reprezentuje jednostkę już w zestawu wyników dla tego zapytania
      * Jeśli tak, zostanie zwrócony istniejącej jednostki <sup>(1)</sup>
      * W przeciwnym razie nowy obiekt zostanie utworzona i zwrócona

<sup>(1) </sup> Nie ma zapytań, śledzenie użycia słabe odwołania do śledzenia jednostek, które już zostały zwrócone. Jeśli poprzedni wynik o tej samej tożsamości wykracza poza zakres i wyrzucanie elementów bezużytecznych działa, możesz uzyskać nowego wystąpienia jednostki.

## <a name="when-queries-are-executed"></a>Podczas wykonywania kwerendy

Podczas wywoływania LINQ operatory są po prostu budowania reprezentacji w pamięci zapytania. Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy wyniki są używane.

Najbardziej typowe operacje, które powoduje zapytania są wysyłane do bazy danych są:
* Iteracja wyniki w `for` pętli
* Przy użyciu operatora, takich jak `ToList`, `ToArray`, `Single`,`Count`
* Element dataBinding wyników zapytania do interfejsu użytkownika

> [!WARNING]  
> **Sprawdzanie poprawności danych wejściowych użytkownika:** podczas EF zapewnia ochronę przed atakami iniekcji kodu SQL, nie ma żadnych ogólne sprawdzania poprawności danych wejściowych. W związku z tym jeśli wartości były przekazywane do interfejsów API, które są używane w zapytaniach LINQ, przypisane do właściwości obiektu itp., pochodzi z niezaufanego źródła, a następnie odpowiednią sprawdzania poprawności, zgodnie z wymaganiami aplikacji należy wykonać. W tym wszystkie dane wejściowe użytkownika używane do dynamicznego tworzenia zapytania. Nawet w przypadku używania LINQ, jeśli akceptujesz wprowadzenia danych przez użytkownika do kompilacji wyrażeń, które należy się upewnić, niż tylko danego wyrażenia może być skonstruowany.
