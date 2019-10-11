---
title: Nieprzetworzone zapytania SQL — EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 33601d570fa0b7a1fcada1705843da3798c00094
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181966"
---
# <a name="raw-sql-queries"></a>Pierwotne zapytania SQL

Entity Framework Core pozwala na rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjną bazą danych. Surowe zapytania SQL są przydatne, jeśli nie można wyrazić tego zapytania przy użyciu LINQ. Nieprzetworzone zapytania SQL są używane również wtedy, gdy użycie zapytania LINQ skutkuje nieefektywnym zapytaniem SQL. Surowe zapytania SQL mogą zwracać zwykłe typy jednostek lub [mniejsze typy jednostek](xref:core/modeling/keyless-entity-types) , które są częścią modelu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/RawSQL/Sample.cs) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="basic-raw-sql-queries"></a>Podstawowe nieprzetworzone zapytania SQL

Można użyć metody rozszerzenia `FromSqlRaw`, aby rozpocząć zapytanie LINQ na podstawie pierwotnego zapytania SQL. `FromSqlRaw` można używać tylko w przypadku katalogów głównych, które są bezpośrednio w `DbSet<>`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

W celu wykonania procedury składowanej można użyć nieprzetworzonych zapytań SQL.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Przekazywanie parametrów

> [!WARNING]
> **Zawsze używaj parametryzacja dla nieprzetworzonych zapytań SQL**
>
> Wprowadzając wszelkie wartości podane przez użytkownika w nieprzetworzonej kwerendzie SQL, należy zachować ostrożność, aby uniknąć ataków iniekcji SQL. Oprócz sprawdzania, czy takie wartości nie zawierają nieprawidłowych znaków, należy zawsze używać parametryzacja, które wysyła wartości odrębnie od tekstu SQL.
>
> W szczególności nigdy nie przekazuj ciągu połączonego lub interpolowanego (`$""`) z niezweryfikowanymi wartościami dostarczonymi przez użytkownika do `FromSqlRaw` lub `ExecuteSqlRaw`. Metody `FromSqlInterpolated` i `ExecuteSqlInterpolated` umożliwiają używanie składni interpolacji ciągów w taki sposób, aby chronić przed atakami polegającymi na iniekcji SQL.

Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej, dołączając symbol zastępczy parametru w ciągu zapytania SQL i dostarczając dodatkowy argument. Ta składnia może wyglądać podobnie do składni `String.Format`, podana wartość jest opakowana w `DbParameter` i wygenerowanej nazwy parametru, gdzie został określony symbol zastępczy `{0}`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated` jest podobna do `FromSqlRaw`, ale umożliwia użycie składni interpolacji ciągów. Podobnie jak `FromSqlRaw`, `FromSqlInterpolated` można używać tylko w przypadku katalogów głównych zapytań. Podobnie jak w poprzednim przykładzie, wartość jest konwertowana na `DbParameter` i nie jest narażony na wstrzyknięcie kodu SQL.

> [!NOTE]
> Przed wersjami 3,0 `FromSqlRaw` i `FromSqlInterpolated` były dwa przeciążenia o nazwach `FromSql`. Aby uzyskać więcej informacji, zobacz [sekcję poprzednie wersje](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Możesz również skonstruować DbParameter i podać go jako wartość parametru. Ponieważ jest używany zwykły symbol zastępczy parametru SQL, a nie symbol zastępczy ciągu, `FromSqlRaw` można bezpiecznie użyć:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw` pozwala używać parametrów nazwanych w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Tworzenie za pomocą LINQ

Możesz złożyć na początku wstępnego nieprzetworzonego zapytania SQL przy użyciu operatorów LINQ. EF Core traktuje ją jako podzapytanie i redaguje ją w bazie danych. Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF). A następnie redaguje przy użyciu LINQ, aby przeprowadzić filtrowanie i sortowanie.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Powyższe zapytanie generuje następujący kod SQL:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>Uwzględnianie danych pokrewnych

Metoda `Include` może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

Redagowanie przy użyciu LINQ wymaga, aby pierwotne zapytanie SQL było możliwe do utworzenia, ponieważ EF Core będzie traktować dane SQL jako podzapytanie. Zapytania SQL, które mogą być składane na początku ze słowem kluczowym `SELECT`. Dodatkowo przekazanie danych SQL nie powinno zawierać żadnych znaków ani opcji, które nie są prawidłowe w podzapytaniu, na przykład:

- Końcowy średnik
- Na SQL Server, końcowa Wskazówka na poziomie zapytania (na przykład `OPTION (HASH JOIN)`)
- Na SQL Server, klauzula `ORDER BY`, która nie jest używana z `OFFSET 0` lub `TOP 100 PERCENT` w klauzuli `SELECT`

SQL Server nie zezwala na tworzenie wywołań procedur składowanych, więc każda próba zastosowania dodatkowych operatorów zapytań do takiego wywołania spowoduje nieprawidłowe użycie języka SQL. Użyj metody `AsEnumerable` lub `AsAsyncEnumerable` bezpośrednio po `FromSqlRaw` lub `FromSqlInterpolated` metodach, aby upewnić się, że EF Core nie próbuje utworzyć przez procedurę składowaną.

## <a name="change-tracking"></a>Śledzenie zmian

Zapytania, które używają metod `FromSqlRaw` lub `FromSqlInterpolated`, są zgodne ze szczegółowymi regułami śledzenia zmian, co inne zapytanie LINQ w EF Core. Na przykład, jeśli typ jednostki projekty zapytań, wyniki będą śledzone domyślnie.

Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie wyłącza śledzenie zmian z wywołaniem `AsNoTracking`:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Ograniczenia

Istnieje kilka ograniczeń, które należy wziąć pod uwagę podczas korzystania z nieprzetworzonych zapytań SQL:

- Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki.
- Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości. Należy zauważyć, że to zachowanie różni się od EF6. EF6 zignorował właściwość do mapowania kolumn dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały pasować do nazw właściwości.
- Zapytanie SQL nie może zawierać powiązanych danych. Jednak w wielu przypadkach można utworzyć na górze zapytania przy użyciu operatora `Include`, aby zwrócić powiązane dane (zobacz [m.in. powiązane dane](#including-related-data)).

## <a name="previous-versions"></a>Poprzednie wersje

EF Core w wersji 2,2 i starszych miała dwa przeciążenia metody o nazwie `FromSql`, które zadziałały w taki sam sposób, jak w przypadku nowszych `FromSqlRaw` i `FromSqlInterpolated`. Można łatwo przypadkowo wywołać metodę nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie. Przypadkowe wywołanie nieprawidłowego przeciążenia może spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.
