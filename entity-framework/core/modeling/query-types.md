---
title: Typy zapytań - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a>Typy zapytań
> [!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.1

Oprócz typy jednostek, model EF Core może zawierać _typy zapytań_, które mogą służyć do wykonywania kwerend bazy danych w odniesieniu do danych, który nie jest zamapowany na typy jednostek.

Typy zapytań mają wiele podobieństw z typami jednostek:

- Ich można również dodać do modelu albo w `OnModelCreating`, lub za pośrednictwem właściwości "set" na pochodnym _DbContext_.
- Obsługuje wiele mapowanie funkcji, takich jak dziedziczenia mapowania właściwości nawigacji (zobacz ograniczenia poniżej), a relacyjne magazynów, możliwość konfigurowania obiektów bazy danych docelowych i kolumn za pomocą metody interfejsu API fluent lub adnotacji danych.

Jednak są one różne od podmiotu typy w tym ich:

- Nie wymagają klucza ma zostać zdefiniowana.
- Nigdy nie są śledzone zmiany na _DbContext_ i w związku z tym nigdy nie wstawienia, zaktualizowane lub usunięte w bazie danych.
- Nigdy nie są wykrywane przez Konwencję.
- Obsługują tylko podzestaw możliwości mapowania nawigacji — w szczególności, nigdy nie mogą one działać jako główny koniec relacji.
- Są opisane na _element ModelBuilder_ przy użyciu `Query` metody zamiast `Entity` metody.
- Są mapowane na _DbContext_ za pośrednictwem właściwości typu `DbQuery<T>` zamiast `DbSet<T>`
- Są mapowane do obiektów bazy danych przy użyciu `ToView` metody, a nie `ToTable`.
- Mogą być mapowane na _kwerendy_ — Definiowanie zapytanie jest zapytaniem dodatkowej zadeklarowany w modelu, który działa źródła danych dla typu zapytania.

Niektóre scenariusze użycia główne typy zapytań to:

- Służy jako typ zwracany dla ad hoc `FromSql()` zapytania.
- Mapowanie do widoków bazy danych.
- Mapowanie do tabel, które nie ma zdefiniowanego klucza podstawowego.
- Mapowanie do zapytań, zdefiniowanego w modelu.

> [!TIP]
> Mapowanie typu zapytania do obiektu bazy danych jest osiągane przy użyciu `ToView` interfejsu API fluent. Z perspektywy EF Core obiekt bazy danych określony w ramach tej metody jest _widoku_, co oznacza, że jest ona traktowana jako źródła zapytań tylko do odczytu i nie może być elementem docelowym aktualizacji, Wstaw lub usuwanie operacji. Jednak nie oznacza to, że obiektu bazy danych jest faktycznie musi być widoku bazy danych — mogą być alternatywnie tabeli bazy danych, które będą traktowane jako tylko do odczytu. Z drugiej strony, dla typów jednostek EF Core przyjęto założenie, że obiekt bazy danych określona w `ToTable` metoda może być traktowana jako _tabeli_, co oznacza, że można użyć jako źródła zapytań, ale może wskazywane przez aktualizację, usuwanie i wstawianie operacje. W rzeczywistości należy określić nazwę widoku bazy danych w `ToTable` i wszystko powinny działać prawidłowo tak długo, jak widok jest skonfigurowany tak, aby nadaje się do aktualizacji w bazie danych.

## <a name="example"></a>Przykład

Poniższy przykład przedstawia użycie typu zapytania zbadać widok bazy danych.

> [!TIP]
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) w witrynie GitHub.

Najpierw należy zdefiniować modelu prostego blogu i Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

Następnie określ widok proste bazy danych, który pozwoli nam się zapytanie o liczbę wpisów skojarzone z każdym blogu:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

Następnie określ klasę do przechowywania wyników z widoku bazy danych:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu `modelBuilder.Query<T>` interfejsu API.
Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Na koniec mamy kwerendy widoku bazy danych w standardowy sposób:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Należy zauważyć, że firma Microsoft również zdefiniowanych właściwości poziomu zapytania kontekstu (DbQuery) do działania jako katalog główny dla zapytań dotyczących tego typu.
