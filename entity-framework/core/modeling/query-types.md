---
title: "Typy zapytań - EF Core"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a>Typy zapytań
> [!NOTE]
> Ta funkcja jest nowa w programie EF Core 2.1

Typy zapytań są typy wyników zapytania tylko do odczytu do dodania do modelu EF Core. Typy zapytań włączenia zapytań ad hoc (np. typy anonimowe), ale są bardziej elastyczne, ponieważ mają one określona konfiguracja mapowania.

Są one podobny do typów jednostek w tym:

- Są one typy obiektów POCO C#, które są dodawane do modelu w ```OnModelCreating``` przy użyciu ```ModelBuilder.Query``` metody, lub za pomocą właściwości DbContext "set" (dla zapytania typy taka właściwość jest typu ```DbQuery<T>``` zamiast ```DbSet<T>```).
- Obsługuje wiele funkcji mapowania jako typy jednostek regularne. Na przykład mapowania dziedziczenia, nawigacji (zobacz poniżej limitiations) i w sklepach relacyjne, możliwość konfigurowania obiektów schematu docelowej bazy danych za pośrednictwem ```ToTable```, ```HasColumn``` metod fluent api (lub adnotacji danych).

Typy zapytań różnią się od podmiotu typy w tym ich:

- Nie wymagają klucza ma zostać zdefiniowana.
- Nigdy nie są śledzone przez obiekt śledzący zmiany.
- Nigdy nie są wykrywane przez Konwencję.
- Obsługują tylko podzestaw możliwości mapowania nawigacji — w szczególności, nigdy nie mogą one działać jako główny koniec relacji.
- Mogą być mapowane na _kwerendy_ -A Definiowanie zapytanie jest zapytaniem dodatkowej, będący źródłem danych dla typu zapytania.

Niektóre scenariusze użycia główne typy zapytań to:

- Mapowanie do widoków bazy danych.
- Mapowanie do tabel, które nie ma zdefiniowanego klucza podstawowego.
- Służy jako typ zwracany dla ad hoc ```FromSql()``` zapytania.
- Mapowanie do zapytań, zdefiniowanego w modelu.

> [!TIP]
> Mapowanie typu zapytania do widoku bazy danych jest osiągane przy użyciu ```ToTable``` interfejsu API fluent.

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

Firma Microsoft Skonfiguruj typ zapytania w _OnModelCreating_ przy użyciu ```modelBuilder.Query<T>``` interfejsu API.
Używamy standardowej konfiguracji fluent API do skonfigurowania mapowania dla typu zapytania:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Na koniec mamy kwerendy widoku bazy danych w standardowy sposób:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Należy zauważyć, że firma Microsoft również zdefiniowanych właściwości poziomu zapytania kontekstu (DbQuery) do działania jako katalog główny dla zapytań dotyczących tego typu.
