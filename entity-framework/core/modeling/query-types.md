---
title: Typy zapytań — EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 6f0f860c6a4e619e13d55e6207234a8b5261ee09
ms.sourcegitcommit: d1230e34673b8323a227ab37958dfa77f3684728
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68330798"
---
# <a name="query-types"></a>Typy zapytań
> [!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.1

Oprócz typów jednostek, mogą zawierać model programu EF Core _typy zapytań_, który może służyć do przeprowadzania zapytań bazy danych dla danych, które nie są mapowane na typy jednostek.

## <a name="compare-query-types-to-entity-types"></a>Porównanie typów zapytań do typów jednostek

Typy zapytań są podobne do typów jednostek, w tym ich:

- Można dodać do modelu albo w `OnModelCreating` lub za pośrednictwem "set" właściwości pochodnej _DbContext_.
- Obsługuje wiele tych samych funkcji mapowania, takich jak właściwości mapowania i nawigację dziedziczenia. W sklepach relacyjnych można skonfigurować obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.

Jednak różnią się one od jednostki typy w tym ich:

- Nie wymagają klucza do zdefiniowania.
- Nigdy nie są śledzone zmiany na _DbContext_ i w związku z tym są nigdy nie wstawione, zaktualizowane lub usunięte w bazie danych.
- Nigdy nie są odnajdywane przez Konwencję.
- Tylko obsługują podzbiór funkcji map nawigacji — w szczególności:
  - Nigdy nie mogą one działać jako główny koniec relacji.
  - Może zawierać tylko właściwości nawigacji odwołania, wskazując jednostek.
  - Jednostki nie może zawierać właściwości nawigacji, aby typy zapytań.
- Szybkiego reagowania na _element ModelBuilder_ przy użyciu `Query` metody zamiast `Entity` metody.
- Są mapowane na _DbContext_ za pośrednictwem właściwości typu `DbQuery<T>` zamiast `DbSet<T>`
- Są mapowane na obiekty bazy danych przy użyciu `ToView` metody, zamiast `ToTable`.
- Mogą być mapowane na _kwerendy_ — Definiowanie zapytanie jest zapytaniem dodatkowej zadeklarowanych w modelu, który działa źródła danych dla typu zapytania.

## <a name="usage-scenarios"></a>Scenariusze użytkowania

Niektóre scenariusze użycia główne typy zapytań to:

- Służy jako typ zwracany dla ad hoc `FromSql()` zapytania.
- Mapowanie na widoki baz danych.
- Mapowania tabel, które nie mają zdefiniowany klucz podstawowy.
- Mapowanie do zapytań zdefiniowanych w modelu.

## <a name="mapping-to-database-objects"></a>Mapowanie na obiekty bazy danych

Mapowanie typu zapytania do obiektu bazy danych jest osiągane przy użyciu `ToView` wygodnego interfejsu API. Z perspektywy programu EF Core jest podany w tej metodzie obiekt bazy danych _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, wstawiania lub operacje usuwania. Jednak oznacza to, że obiekt bazy danych jest faktycznie wymagana do widoku bazy danych — może też być tabeli bazy danych, które będą traktowane jako tylko do odczytu. Z drugiej strony, dla typów jednostek programu EF Core przyjęto założenie, że obiektu bazy danych określony w `ToTable` metoda może być traktowana jako _tabeli_, co oznacza, że mogą być używane jako źródło zapytania, ale wskazywane przez aktualizację, usuwanie i wstawianie operacje. W rzeczywistości, można określić nazwy widoku bazy danych w `ToTable` i wszystko powinno działać prawidłowo tak długo, jak widok jest skonfigurowany jako nadaje się do aktualizacji w bazie danych.

## <a name="example"></a>Przykład

Poniższy przykład pokazuje, jak zapytania widoku bazy danych za pomocą typu zapytania.

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) użyty w tym artykule można zobaczyć w witrynie GitHub.

Najpierw należy zdefiniować prosty model blogu i Post:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

Następnie zdefiniuj widok prostej bazy danych, który umożliwi nam zapytania liczby stanowisk skojarzonych z każdym blog:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

Następnie zdefiniuj klasę zawierającą wyniki z widoku bazy danych:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu `modelBuilder.Query<T>` interfejsu API.
Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

Następnie skonfigurujemy `DbContext` , aby `DbQuery<T>`uwzględnić:[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]

Na koniec mamy zapytania widoku bazy danych w sposób standardowy:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> Należy zauważyć, że również zdefiniowaliśmy właściwości zapytania poziom kontekstu (DbQuery) do działania jako główne zapytań dla tego typu.
