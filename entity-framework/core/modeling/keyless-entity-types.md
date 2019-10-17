---
title: Typy jednostek o mniejszej długości — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 3dbc2700fc9bb277eb90885dfc2506c250ae21f1
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445935"
---
# <a name="keyless-entity-types"></a>Typy jednostek bez kluczy

> [!NOTE]
> Ta funkcja została dodana w EF Core 2,1 pod nazwą typów zapytań. W EF Core 3,0 nazwa koncepcji została zmieniona na typy jednostek, które nie są mniejsze.

Oprócz zwykłych typów jednostek model EF Core może zawierać _typy jednostek_bez kluczy, które mogą być używane do przeprowadzania zapytań bazy danych do danych, które nie zawierają wartości klucza.

## <a name="keyless-entity-types-characteristics"></a>Charakterystyka typów jednostek bez parametrów

Typy jednostek o mniejszej liczbie obsługują wiele takich samych możliwości mapowania jak zwykłe typy jednostek, takie jak mapowanie dziedziczenia i właściwości nawigacji. W magazynach relacyjnych można konfigurować docelowe obiekty i kolumny bazy danych za pomocą metod interfejsu API Fluent lub adnotacji danych.

Jednak różnią się one od zwykłych typów jednostek w tym, że:

- Nie można zdefiniować klucza.
- Nigdy nie są śledzone pod kątem zmian w _kontekście DbContext_ , dlatego nigdy nie są wstawiane, aktualizowane ani usuwane w bazie danych.
- Nigdy nie są odnajdywane według Konwencji.
- Obsługiwane jest tylko podzbiór funkcji mapowania nawigacji, w tym:
  - Mogą nigdy nie działać jako główny koniec relacji.
  - Mogą nie mieć nawigacji do jednostek będących własnością
  - Mogą zawierać tylko właściwości nawigacji referencyjnej wskazujące zwykłe jednostki.
  - Jednostki nie mogą zawierać właściwości nawigacji do typów jednostek, które nie są mniejsze.
- Należy skonfigurować z wywołaniem metody `.HasNoKey()`.
- Może być zamapowany na _zapytanie definiujące_. Zapytanie definiujące jest zapytaniem zadeklarowanym w modelu, który działa jako źródło danych dla typu jednostki o niższym stopniu.

## <a name="usage-scenarios"></a>Scenariusze użytkowania

Niektóre główne scenariusze użycia dla typów jednostek nie są następujące:

- Służy jako typ zwracany dla [nieprzetworzonych zapytań SQL](xref:core/querying/raw-sql).
- Mapowanie do widoków bazy danych, które nie zawierają klucza podstawowego.
- Mapowanie do tabel, które nie mają zdefiniowanego klucza podstawowego.
- Mapowanie na zapytania zdefiniowane w modelu.

## <a name="mapping-to-database-objects"></a>Mapowanie na obiekty bazy danych

Mapowanie typu jednostki o mniejszym stopniu do obiektu bazy danych jest realizowane przy użyciu `ToTable` lub `ToView` interfejsu API Fluent. Z perspektywy EF Core obiekt bazy danych określony w tej metodzie jest _widokiem_, co oznacza, że jest traktowany jako źródło zapytania tylko do odczytu i nie może być elementem docelowym operacji Update, INSERT ani Delete. Nie oznacza to jednak, że obiekt bazy danych jest rzeczywiście wymagany jako widok bazy danych. Może być również tabelą bazy danych, która będzie traktowana jako tylko do odczytu. Z kolei w przypadku regularnych typów jednostek EF Core zakłada, że obiekt bazy danych określony w metodzie `ToTable` może być traktowany jako _tabela_, co oznacza, że może być używany jako źródło zapytania, ale również przeznaczony dla operacji Update, DELETE i Insert. W rzeczywistości można określić nazwę widoku bazy danych w `ToTable`, a wszystko powinno działać prawidłowo, o ile widok jest skonfigurowany do aktualizacji w bazie danych.

> [!NOTE]
> `ToView` zakłada, że obiekt już istnieje w bazie danych i nie zostanie utworzony przez migracje.

## <a name="example"></a>Przykład

Poniższy przykład pokazuje, jak używać typów jednostek bez użycia do wykonywania zapytań w widoku bazy danych.

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) tego artykułu można wyświetlić w witrynie GitHub.

Najpierw zdefiniujemy prosty blog i model post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Następnie definiujemy prosty widok bazy danych, który umożliwi nam wykonywanie zapytań dotyczących liczby wpisów skojarzonych z każdym blogiem:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Następnie zdefiniujemy klasę do przechowywania wyniku z widoku bazy danych:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Następnie skonfigurujemy typ jednostki "Less" w _OnModelCreating_ przy użyciu interfejsu API `HasNoKey`.
Korzystamy z interfejsu API konfiguracji Fluent, aby skonfigurować mapowanie dla typu jednostki less:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Następnie skonfigurujemy `DbContext` w celu uwzględnienia `DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Na koniec możemy zbadać widok bazy danych w standardowy sposób:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Należy zauważyć, że zdefiniowano również właściwość zapytania poziomu kontekstu (Nieogólnymi) do działania jako element główny dla zapytań dla tego typu.
