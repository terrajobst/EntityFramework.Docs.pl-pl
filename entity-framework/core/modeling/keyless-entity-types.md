---
title: Typy jednostek o mniejszej długości — EF Core
description: Jak skonfigurować typy jednostek mniej niż przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 129e24b154ba32583435aeb742dbf478350344e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824664"
---
# <a name="keyless-entity-types"></a>Typy jednostek bez kluczy

> [!NOTE]
> Ta funkcja została dodana w EF Core 2,1 pod nazwą typów zapytań. W EF Core 3,0 nazwa koncepcji została zmieniona na typy jednostek, które nie są mniejsze.

Oprócz zwykłych typów jednostek model EF Core może zawierać _typy jednostek_bez kluczy, które mogą być używane do przeprowadzania zapytań bazy danych do danych, które nie zawierają wartości klucza.

## <a name="keyless-entity-types-characteristics"></a>Charakterystyka typów jednostek bez parametrów

Typy jednostek o mniejszej liczbie obsługują wiele takich samych możliwości mapowania jak zwykłe typy jednostek, takie jak mapowanie dziedziczenia i właściwości nawigacji. W sklepach relacyjnych można skonfigurować obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.

Jednak różnią się one od zwykłych typów jednostek w tym, że:

- Nie można zdefiniować klucza.
- Nigdy nie są śledzone pod kątem zmian w _kontekście DbContext_ , dlatego nigdy nie są wstawiane, aktualizowane ani usuwane w bazie danych.
- Nigdy nie są odnajdywane przez Konwencję.
- Obsługiwane jest tylko podzbiór funkcji mapowania nawigacji, w tym:
  - Nigdy nie mogą one działać jako główny koniec relacji.
  - Mogą nie mieć nawigacji do jednostek będących własnością
  - Mogą zawierać tylko właściwości nawigacji referencyjnej wskazujące zwykłe jednostki.
  - Jednostki nie mogą zawierać właściwości nawigacji do typów jednostek, które nie są mniejsze.
- Należy skonfigurować za pomocą wywołania metody `.HasNoKey()`.
- Może być zamapowany na _zapytanie definiujące_. Zapytanie definiujące jest zapytaniem zadeklarowanym w modelu, który działa jako źródło danych dla typu jednostki o niższym stopniu.

## <a name="usage-scenarios"></a>Scenariusze użytkowania

Niektóre główne scenariusze użycia dla typów jednostek nie są następujące:

- Służy jako typ zwracany dla [nieprzetworzonych zapytań SQL](xref:core/querying/raw-sql).
- Mapowanie do widoków bazy danych, które nie zawierają klucza podstawowego.
- Mapowania tabel, które nie mają zdefiniowany klucz podstawowy.
- Mapowanie do zapytań zdefiniowanych w modelu.

## <a name="mapping-to-database-objects"></a>Mapowanie na obiekty bazy danych

Mapowanie typu jednostki o mniejszym stopniu do obiektu bazy danych jest realizowane przy użyciu `ToTable` lub `ToView` interfejsu API Fluent. Z perspektywy programu EF Core jest podany w tej metodzie obiekt bazy danych _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, wstawiania lub operacje usuwania. Nie oznacza to jednak, że obiekt bazy danych jest rzeczywiście wymagany jako widok bazy danych. Może być również tabelą bazy danych, która będzie traktowana jako tylko do odczytu. W przypadku zwykłych typów jednostek EF Core zakłada, że obiekt bazy danych określony w metodzie `ToTable` może być traktowany jako _tabela_, co oznacza, że może być używany jako źródło zapytania, ale również celem operacji Update, DELETE i Insert. W rzeczywistości, można określić nazwy widoku bazy danych w `ToTable` i wszystko powinno działać prawidłowo tak długo, jak widok jest skonfigurowany jako nadaje się do aktualizacji w bazie danych.

> [!NOTE]
> `ToView` zakłada, że obiekt już istnieje w bazie danych i nie zostanie utworzony przez migracje.

## <a name="example"></a>Przykład

Poniższy przykład pokazuje, jak używać typów jednostek bez użycia do wykonywania zapytań w widoku bazy danych.

> [!TIP]
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) użyty w tym artykule można zobaczyć w witrynie GitHub.

Najpierw należy zdefiniować prosty model blogu i Post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Następnie zdefiniuj widok prostej bazy danych, który umożliwi nam zapytania liczby stanowisk skojarzonych z każdym blog:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Następnie zdefiniuj klasę zawierającą wyniki z widoku bazy danych:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Następnie skonfigurujemy typ jednostki mniej _OnModelCreating_ przy użyciu interfejsu API `HasNoKey`.
Korzystamy z interfejsu API konfiguracji Fluent, aby skonfigurować mapowanie dla typu jednostki less:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Następnie skonfigurujemy `DbContext` w taki sposób, aby obejmował `DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Na koniec mamy zapytania widoku bazy danych w sposób standardowy:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Należy zauważyć, że zdefiniowano również właściwość zapytania poziomu kontekstu (Nieogólnymi) do działania jako element główny dla zapytań dla tego typu.
