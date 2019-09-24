---
title: Nieprzetworzone zapytania SQL — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ebec5775770c0f1e297eaaf35bf644c605a69afc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197775"
---
# <a name="raw-sql-queries"></a>Pierwotne zapytania SQL

Entity Framework Core pozwala na rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjną bazą danych. Może to być przydatne, jeśli zapytanie, które chcesz wykonać, nie może być wyrażone przy użyciu LINQ, lub jeśli użycie zapytania LINQ jest wynikiem niewydajnego zapytania SQL. Surowe zapytania SQL mogą zwracać zwykłe typy jednostek lub [mniejsze typy jednostek](xref:core/modeling/keyless-entity-types) , które są częścią modelu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="basic-raw-sql-queries"></a>Podstawowe nieprzetworzone zapytania SQL

Możesz użyć metody rozszerzenia `FromSqlRaw` , aby rozpocząć zapytanie LINQ na podstawie pierwotnego zapytania SQL.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

W celu wykonania procedury składowanej można użyć nieprzetworzonych zapytań SQL.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Przekazywanie parametrów

> [!WARNING]
> **Zawsze używaj parametryzacja dla nieprzetworzonych zapytań SQL**
>
> Wprowadzając wszelkie wartości podane przez użytkownika w nieprzetworzonej kwerendzie SQL, należy zachować ostrożność, aby uniknąć ataków iniekcji SQL. Oprócz sprawdzania, czy takie wartości nie zawierają nieprawidłowych znaków, należy zawsze używać parametryzacja, które wysyła wartości odrębnie od tekstu SQL.
>
> W szczególności nigdy nie przekazuj połączonego lub interpolowanego ciągu (`$""`) z niezweryfikowanymi wartościami dostarczonymi przez użytkownika do `FromSqlRaw` lub. `ExecuteSqlRaw` Metody `FromSqlInterpolated` i`ExecuteSqlInterpolated` umożliwiają używanie składni interpolacji ciągów w sposób chroniący przed atakami polegającymi na iniekcji SQL.

Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej, dołączając symbol zastępczy parametru w ciągu zapytania SQL i dostarczając dodatkowy argument. Chociaż może to wyglądać podobnie `String.Format` do składni, podana wartość jest opakowana `DbParameter` w i wygenerowana nazwa `{0}` parametru, gdzie symbol zastępczy został określony.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Jako alternatywę dla `FromSqlRaw`, można użyć `FromSqlInterpolated` , która umożliwia bezpieczne korzystanie z interpolacji ciągów. Tak jak w poprzednim przykładzie, wartość jest konwertowana na `DbParameter` i dlatego nie jest narażona na wstrzyknięcie kodu SQL:

> [!NOTE]
> Przed wersjami 3,0 `FromSqlRaw` i `FromSqlInterpolated` były dwa przeciążenia o nazwie `FromSql`. Zobacz [sekcję poprzednie wersje](#previous-versions) , aby uzyskać więcej informacji.


<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Możesz również skonstruować DbParameter i podać go jako wartość parametru. Ponieważ jest używany zwykły symbol zastępczy parametru SQL, a nie symbol zastępczy `FromSqlRaw` ciągu, można bezpiecznie użyć:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Pozwala to używać parametrów nazwanych w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Tworzenie za pomocą LINQ

Jeśli zapytanie SQL może składać się z bazy danych, możesz utworzyć na początku pierwotnego, nieprzetworzonego zapytania SQL przy użyciu operatorów LINQ. Zapytania SQL, które mogą być składane na początku `SELECT` ze słowem kluczowym.

W poniższym przykładzie użyto nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie tworzy je przy użyciu LINQ do wykonywania filtrowania i sortowania.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Spowoduje to wygenerowanie następującej kwerendy SQL:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Śledzenie zmian

Zapytania, które używają `FromSql` metod, są zgodne ze szczegółowymi regułami śledzenia zmian co inne zapytania LINQ w EF Core. Na przykład, jeśli typ jednostki projekty zapytań, wyniki będą śledzone domyślnie.

Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie wyłącza śledzenie zmian z wywołaniem `AsNoTracking`:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Uwzględnianie danych pokrewnych

`Include` Metoda może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Należy pamiętać, że wymaga to utworzenia pierwotnego zapytania SQL; nie będzie to szczególnie możliwe w przypadku wywołań procedur przechowywanych. Zapoznaj się z uwagami dotyczącymi możliwości tworzenia w obszarze [ograniczenia](#limitations)).

## <a name="limitations"></a>Ograniczenia

Istnieje kilka ograniczeń, które należy wziąć pod uwagę podczas korzystania z nieprzetworzonych zapytań SQL:

* Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki.

* Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości. Należy zauważyć, że różni się od EF6, gdzie mapowanie właściwości i kolumn zostało zignorowane dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały pasować do nazw właściwości.

* Zapytanie SQL nie może zawierać powiązanych danych. Jednak w wielu przypadkach można utworzyć na górze zapytania przy użyciu operatora, `Include` aby zwrócić powiązane dane (zobacz m.in. [powiązane dane](#including-related-data)).

* `SELECT`instrukcje przesłane do tej metody powinny być zwykle możliwe do przesłania: Jeśli EF Core musi oszacować dodatkowe operatory zapytań na serwerze (na przykład w celu przetłumaczenia operatorów LINQ zastosowanych po `FromSql` metodach), podana wartość SQL będzie traktowana jako podzapytanie. Oznacza to, że przesłany kod SQL nie powinien zawierać żadnych znaków ani opcji, które nie są prawidłowe w podzapytaniu, na przykład:
  * końcowy średnik
  * Na SQL Server, końcowa Wskazówka na poziomie zapytania (na przykład `OPTION (HASH JOIN)`)
  * Na SQL Server, `ORDER BY` klauzula, do której nie dołączono `OFFSET 0` ani `TOP 100 PERCENT` w `SELECT` klauzuli

* Należy pamiętać, że SQL Server nie zezwala na tworzenie wywołań procedur składowanych, więc każda próba zastosowania dodatkowych operatorów zapytań do takiego wywołania spowoduje nieprawidłowe użycie języka SQL. Operatory zapytań można wprowadzać po `AsEnumerable()` przeprowadzeniu oceny klienta.

# <a name="previous-versions"></a>Poprzednie wersje

EF Core wersja 2,2 i wcześniejsze mają dwa przeciążenia o `FromSql` nazwach, które zachowują się w taki sam `FromSqlRaw` sposób `FromSqlInterpolated`jak nowsze i. Dzięki temu bardzo łatwo można przypadkowo wywołać metodę nieprzetworzonego ciągu, gdy zamiarem było wywołanie interpolowanej metody String i odwrotnie. Może to spowodować, że kwerendy nie są sparametryzowane, gdy powinny one być.
