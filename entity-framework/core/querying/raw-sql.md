---
title: Nieprzetworzone zapytania SQL — EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417671"
---
# <a name="raw-sql-queries"></a>Pierwotne zapytania SQL

Entity Framework Core umożliwia rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjnej bazy danych. Nieprzetworzone zapytania SQL są przydatne, jeśli kwerenda, której nie można wyrazić za pomocą LINQ. Nieprzetworzone kwerendy SQL są również używane, jeśli przy użyciu kwerendy LINQ powoduje nieefektywne zapytanie SQL. Nieprzetworzone kwerendy SQL mogą zwracać zwykłe typy jednostek lub [typy jednostek bezkluszeń,](xref:core/modeling/keyless-entity-types) które są częścią modelu.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) przykład artykułu na GitHub.

## <a name="basic-raw-sql-queries"></a>Podstawowe nieprzetworzone zapytania SQL

Za pomocą `FromSqlRaw` metody rozszerzenia można rozpocząć kwerendę LINQ opartą na nieprzetworzonej kwerendzie SQL. `FromSqlRaw`mogą być używane tylko na korzeniach zapytań, które są bezpośrednio na . `DbSet<>`

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Nieprzetworzone zapytania SQL mogą być używane do wykonywania procedury składowanej.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Przekazywanie parametrów

> [!WARNING]
> **Zawsze używaj parametryzacji dla nieprzetworzonych zapytań SQL**
>
> Podczas wprowadzania żadnych wartości dostarczonych przez użytkownika do nieprzetworzonej kwerendy SQL, należy zwrócić uwagę, aby uniknąć ataków iniekcji SQL. Oprócz sprawdzania poprawności, że takie wartości nie zawierają nieprawidłowych znaków, zawsze należy użyć parametryzacji, która wysyła wartości oddzielone od tekstu SQL.
>
> W szczególności nigdy nie należy przekazywać ciągu`$""`łączonego lub interpolowanego `FromSqlRaw` ( `ExecuteSqlRaw`) z niezatwierdzonymi wartościami dostarczonymi przez użytkownika do lub . Metody `FromSqlInterpolated` `ExecuteSqlInterpolated` i umożliwiają użycie składni interpolacji ciągów w sposób, który chroni przed atakami iniekcji SQL.

Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej, dołączając symbol zastępczy parametru w ciągu zapytania SQL i zapewniając dodatkowy argument. Chociaż ta składnia może `String.Format` wyglądać jak składnia, podana `DbParameter` wartość jest zawijana `{0}` w a, a wygenerowana nazwa parametru wstawiona w miejscu, w którym określono symbol zastępczy.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`jest podobny `FromSqlRaw` do ale pozwala na użycie składni interpolacji ciągów. Podobnie `FromSqlRaw`jak `FromSqlInterpolated` , może być używany tylko na korzenie kwerendy. Podobnie jak w poprzednim przykładzie, wartość `DbParameter` jest konwertowana na i nie jest narażony na iniekcję SQL.

> [!NOTE]
> Przed wersją 3.0 i `FromSqlRaw` `FromSqlInterpolated` były dwa `FromSql`przeciążenia o nazwie . Aby uzyskać więcej informacji, zobacz [sekcję Poprzednie wersje](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Można również skonstruować DbParameter i podać go jako wartość parametru. Ponieważ używany jest zwykły symbol zastępczy parametru SQL, a nie symbol zastępczy ciągu, `FromSqlRaw` może być bezpiecznie używany:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`umożliwia użycie nazwanych parametrów w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Komponowanie z LINQ

Można skomponować na początku początkowej nieprzetworzonej kwerendy SQL przy użyciu operatorów LINQ. EF Core będzie traktować go jako podkwerendy i komponować nad nim w bazie danych. W poniższym przykładzie użyto nieprzetworzonej kwerendy SQL, która wybiera z funkcji wycenianej w tabeli (TVF). A następnie komponuje się na nim za pomocą LINQ do filtrowania i sortowania.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Powyższa kwerenda generuje następujące SQL:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>W tym powiązane dane

Metoda `Include` może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

Tworzenie z LINQ wymaga nieprzetworzonej kwerendy SQL do komponowania, ponieważ EF Core będzie traktować dostarczony SQL jako podkwerendę. Kwerendy SQL, które mogą być `SELECT` składane na początku od słowa kluczowego. Ponadto sql przekazywane nie powinny zawierać żadnych znaków lub opcji, które nie są prawidłowe w podkwerendy, takich jak:

- Średnik spływowy
- Na programie SQL Server końcowa wskazówka na `OPTION (HASH JOIN)`poziomie kwerendy (na przykład)
- W programie SQL `ORDER BY` Server: klauzula, `OFFSET 0` `TOP 100 PERCENT` która `SELECT` nie jest używana z or w klauzuli

SQL Server nie zezwala na tworzenie za pośrednictwem wywołań procedur składowanych, więc każda próba zastosowania dodatkowych operatorów zapytań do takiego wywołania spowoduje nieprawidłowy SQL. Użyj `AsEnumerable` `AsAsyncEnumerable` lub metody `FromSqlRaw` `FromSqlInterpolated` zaraz po lub metody, aby upewnić się, że EF Core nie próbuje komponować za pomocą procedury składowanej.

## <a name="change-tracking"></a>Śledzenie zmian

Kwerendy, które `FromSqlRaw` `FromSqlInterpolated` używają lub metody wykonaj dokładnie te same reguły śledzenia zmian, jak inne zapytania LINQ w EF Core. Na przykład jeśli kwerendy projekty typów jednostek, wyniki będą śledzone domyślnie.

W poniższym przykładzie użyto nieprzetworzonej kwerendy SQL, która wybiera z funkcji `AsNoTracking`tvf (Table-Valued Function), a następnie wyłącza śledzenie zmian za pomocą połączenia do:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Ograniczenia

Istnieje kilka ograniczeń, o których należy pamiętać podczas korzystania z nieprzetworzonych zapytań SQL:

- Kwerenda SQL musi zwracać dane dla wszystkich właściwości typu jednostki.
- Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości. Należy zauważyć, że to zachowanie różni się od EF6. EF6 zignorował właściwość do mapowania kolumn dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały być zgodne z nazwami właściwości.
- Kwerenda SQL nie może zawierać powiązanych danych. Jednak w wielu przypadkach można skomponować na `Include` górze kwerendy za pomocą operatora do zwracania powiązanych danych (zobacz [Uwzględnienie powiązanych danych](#including-related-data)).

## <a name="previous-versions"></a>Poprzednie wersje

EF Core w wersji 2.2 i wcześniej `FromSql`miał dwa przeciążenia metody o `FromSqlRaw` nazwie, który zachowywał się w taki sam sposób jak nowsze i `FromSqlInterpolated`. Łatwo było przypadkowo wywołać metodę ciągu nieprzetworzonego, gdy intencją było wywołanie interpolowanej metody ciągu, a na odwrót. Wywołanie nieprawidłowe przeciążenie przypadkowo może spowodować kwerendy nie są parametryzowane, kiedy powinny być.
