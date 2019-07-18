---
title: Nieprzetworzone zapytania SQL — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 91592ea9f7c73f10446993282c1874c852000871
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306543"
---
# <a name="raw-sql-queries"></a>Nieprzetworzone zapytania SQL

Entity Framework Core pozwala na rozwijanie do nieprzetworzonych zapytań SQL podczas pracy z relacyjną bazą danych. Może to być przydatne, jeśli zapytanie, które chcesz wykonać, nie może być wyrażone przy użyciu LINQ, lub jeśli użycie zapytania LINQ jest wynikiem nieefektywnych zapytań SQL. Surowe zapytania SQL mogą zwracać typy jednostek lub, rozpoczynając od EF Core 2,1, [typy zapytań](xref:core/modeling/query-types) , które są częścią modelu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="basic-raw-sql-queries"></a>Podstawowe nieprzetworzone zapytania SQL

Możesz użyć metody rozszerzenia *z tabel* , aby rozpocząć zapytanie LINQ na podstawie pierwotnego zapytania SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

W celu wykonania procedury składowanej można użyć nieprzetworzonych zapytań SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Przekazywanie parametrów

Podobnie jak w przypadku dowolnego interfejsu API, który akceptuje SQL, ważne jest, aby Sparametryzuj wszelkie dane wprowadzane przez użytkownika w celu ochrony przed atakami polegającymi na iniekcji SQL. Można dołączać symbole zastępcze parametrów w ciągu zapytania SQL, a następnie podawać wartości parametrów jako dodatkowe argumenty. Wszelkie podawane wartości parametrów zostaną automatycznie przekonwertowane na `DbParameter`.

Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej. Chociaż może to wyglądać podobnie `String.Format` do składni, podana wartość jest opakowana w parametr i wygenerowaną nazwą parametru wstawioną, `{0}` gdzie symbol zastępczy został określony.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

To jest to samo zapytanie, ale przy użyciu składni interpolacji ciągów, która jest obsługiwana w EF Core 2,0 i powyżej:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Możesz również skonstruować DbParameter i podać go jako wartość parametru:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Pozwala to używać parametrów nazwanych w ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma parametry opcjonalne:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Tworzenie za pomocą LINQ

Jeśli zapytanie SQL może składać się z bazy danych, możesz utworzyć na początku pierwotnego, nieprzetworzonego zapytania SQL przy użyciu operatorów LINQ. Zapytania SQL, które mogą być składane na początku `SELECT` ze słowem kluczowym.

W poniższym przykładzie użyto nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie tworzy je przy użyciu LINQ do wykonywania filtrowania i sortowania.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Śledzenie zmian

Zapytania, które używają `FromSql()` dokładnie tych samych reguł śledzenia zmian, jak inne zapytania LINQ w EF Core. Na przykład, jeśli typ jednostki projekty zapytań, wyniki będą śledzone domyślnie.  

Poniższy przykład używa nieprzetworzonego zapytania SQL, które wybiera z funkcji zwracającej tabelę (TVF), a następnie wyłącza śledzenie zmian z wywołaniem. AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Uwzględnianie danych pokrewnych

`Include()` Metoda może służyć do uwzględnienia powiązanych danych, podobnie jak w przypadku innych zapytań LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Ograniczenia

Istnieje kilka ograniczeń, które należy wziąć pod uwagę podczas korzystania z nieprzetworzonych zapytań SQL:

* Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki lub zapytania.

* Nazwy kolumn w zestawie wyników muszą być zgodne z nazwami kolumn, do których są mapowane właściwości. Należy zauważyć, że różni się od EF6, gdzie mapowanie właściwości i kolumn zostało zignorowane dla nieprzetworzonych zapytań SQL i nazwy kolumn zestawu wyników musiały pasować do nazw właściwości.

* Zapytanie SQL nie może zawierać powiązanych danych. Jednak w wielu przypadkach można utworzyć na górze zapytania przy użyciu operatora, `Include` aby zwrócić powiązane dane (zobacz m.in. [powiązane dane](#including-related-data)).

* `SELECT`instrukcje przesłane do tej metody powinny być zwykle możliwe do przesłania: Jeśli EF Core musi oszacować dodatkowe operatory zapytań na serwerze (na przykład w celu przetłumaczenia operatorów LINQ zastosowanych po `FromSql`), podana wartość SQL będzie traktowana jako podzapytanie. Oznacza to, że przesłany kod SQL nie powinien zawierać żadnych znaków ani opcji, które nie są prawidłowe w podzapytaniu, na przykład:
  * końcowy średnik
  * Na SQL Server, końcowa Wskazówka na poziomie zapytania (na przykład `OPTION (HASH JOIN)`)
  * Na SQL Server, `ORDER BY` klauzula, do której nie dołączono `OFFSET 0` ani `TOP 100 PERCENT` w `SELECT` klauzuli

* Instrukcje SQL inne niż `SELECT` są rozpoznawane automatycznie jako nieumożliwiające tworzenie. W związku z tym pełne wyniki procedur składowanych są zawsze zwracane do klienta i wszystkie operatory LINQ zastosowane po `FromSql` zakończeniu są oceniane w pamięci.

> [!WARNING]  
> **Zawsze używaj parametryzacja dla nieprzetworzonych zapytań SQL:** Oprócz sprawdzania poprawności danych wejściowych użytkownika należy zawsze używać parametryzacja dla wszystkich wartości używanych w nieprzetworzonym zapytaniu SQL/poleceniu. Interfejsy API, które akceptują nieprzetworzony ciąg `FromSql` SQL `ExecuteSqlCommand` , takie jak i zezwalają na łatwe przekazywanie wartości jako parametry. `FromSql` Przeciążenia i `ExecuteSqlCommand` akceptujące FormattableString umożliwiają również korzystanie z składni interpolacji ciągów w taki sposób, aby pomóc w ochronie przed atakami polegającymi na iniekcji SQL. 
> 
> Jeśli używasz łączenia ciągów lub interpolacji do dynamicznego kompilowania dowolnej części ciągu zapytania lub przekazywania danych wejściowych użytkownika do instrukcji lub procedur składowanych, które mogą wykonywać te dane wejściowe jako dynamiczny SQL, wówczas użytkownik jest odpowiedzialny za sprawdzanie poprawności wszelkich danych wejściowych Ochrona przed atakami polegającymi na iniekcji SQL.
