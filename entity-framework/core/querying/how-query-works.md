---
title: Jak działają zapytania — EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136239"
---
# <a name="how-queries-work"></a>Jak działają zapytania

Entity Framework Core używa programu Language Integrated Query (LINQ) do wykonywania zapytań dotyczących danych z bazy danych. LINQ umożliwia pisanie kwerend o C# jednoznacznie określonym typie w oparciu o pochodne klasy kontekstowe i jednostki za pomocą (lub języka .NET).

## <a name="the-life-of-a-query"></a>Okres istnienia zapytania

Poniżej znajduje się ogólny przegląd procesu każdego zapytania.

1. Zapytanie LINQ jest przetwarzane przez Entity Framework Core, aby utworzyć reprezentację, która jest gotowa do przetworzenia przez dostawcę bazy danych
   1. Wynik jest buforowany w taki sposób, aby przetwarzanie tego procesu nie było wykonywane za każdym razem, gdy zapytanie jest wykonywane
2. Wynik jest przesyłany do dostawcy bazy danych
   1. Dostawca bazy danych identyfikuje, które części zapytania można ocenić w bazie danych.
   2. Te części zapytania są tłumaczone na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych)
   3. Zapytanie jest wysyłane do bazy danych i zwróconego zestawu wyników (wyniki są wartościami z bazy danych, a nie wystąpieniami jednostek)
3. Dla każdego elementu w zestawie wyników
   1. Jeśli jest to zapytanie śledzenia, program EF sprawdza, czy dane reprezentują jednostkę już w monitorze zmian dla wystąpienia kontekstu
      * Jeśli tak, zostanie zwrócona Istniejąca jednostka
      * Jeśli nie, zostanie utworzona nowa jednostka, śledzenie zmian jest skonfigurowane, a nowa jednostka zostanie zwrócona
   2. Jeśli to nie jest zapytanie śledzenia, Nowa jednostka jest zawsze tworzona i zwracana.

## <a name="when-queries-are-executed"></a>Gdy zapytania są wykonywane

Gdy wywołujesz operatory LINQ, po prostu tworzysz reprezentację w pamięci zapytania. Zapytanie jest wysyłane do bazy danych tylko wtedy, gdy są używane wyniki.

Najczęstsze operacje, które powodują, że zapytanie wysyłane do bazy danych są następujące:

* Iterowanie wyników w pętli `for`
* Używanie operatora, takiego jak `ToList`, `ToArray`, `Single``Count` lub równoważne przeciążenia asynchroniczne

> [!WARNING]  
> **Zawsze Weryfikuj dane wejściowe użytkownika:** Chociaż EF Core chroni przed atakami polegającymi na iniekcji SQL przy użyciu parametrów i literałów ucieczki w zapytaniach, nie weryfikuje danych wejściowych. Odpowiednie sprawdzenie na potrzeby aplikacji należy wykonać, zanim wartości z niezaufanych źródeł są używane w zapytaniach LINQ, przypisane do właściwości jednostki lub przekazane do innych EF Core interfejsów API. Obejmuje to wszelkie dane wejściowe użytkownika używane do dynamicznego konstruowania zapytań. Nawet w przypadku korzystania z LINQ, jeśli akceptujesz dane wprowadzane przez użytkownika w wyrażeniach kompilacji, musisz upewnić się, że można skonstruować tylko zamierzone wyrażenia.
