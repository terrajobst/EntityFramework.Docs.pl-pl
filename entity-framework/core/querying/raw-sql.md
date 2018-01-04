---
title: Zapytania RAW SQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 79894c7b9fd9e40cdf14460abf5d872ee2f4b9f0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="raw-sql-queries"></a>Zapytania SQL pierwotnych

Entity Framework Core pozwala na liście rozwijanej raw kwerendy SQL podczas pracy z relacyjnej bazy danych. Może to być przydatne, jeśli kwerenda, którą chcesz wykonać, nie można wyrazić za pomocą LINQ lub za pomocą zapytań LINQ wynikiem jest nieefektywne SQL wysyłane do bazy danych.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) w witrynie GitHub.

## <a name="limitations"></a>Ograniczenia

Istnieje kilka pod uwagę podczas korzystania z pierwotnych zapytania SQL następujące ograniczenia:
* Zapytania SQL mogą służyć tylko do zwracanych typów jednostki, które są częścią modelu. Brak ulepszeniem na naszych zaległości do [Włącz zwracanie typy ad hoc z pierwotnych zapytania SQL](https://github.com/aspnet/EntityFramework/issues/1862).

* Zapytanie SQL musi zwracać dane dla wszystkich właściwości typu jednostki.

* Nazwy kolumn w zestawie wyników muszą być zgodne nazwy kolumn, które są mapowane właściwości. Należy zauważyć, że ta różni się od EF6 gdzie mapowania właściwości/kolumn została zignorowana dla nieprzetworzonej zapytania SQL i nazw musiały być zgodne z nazwami właściwości kolumny zestawu wyników.

* Zapytania SQL nie może zawierać powiązanych danych. Jednak w wielu przypadkach można utworzyć na podstawie zapytania za pomocą `Include` operatora, aby zwrócić dane dotyczące (zobacz [powiązanych danych w tym](#including-related-data)).

* `SELECT`instrukcje przekazane do tej metody powinno być zazwyczaj zezwala na składanie: Jeśli Core EF należy ocenić operatorów zapytań dodatkowe na serwerze (np. tłumaczenie operatory LINQ zastosowane po `FromSql`), podane SQL będzie traktowany jako podzapytania. Oznacza to, SQL przekazany nie powinna zawierać żadnych znaków ani opcje, które nie są prawidłowe w podzapytaniu, takich jak:
  * końcowe średnikami
  * W programie SQL Server poziom zapytań końcowe wskazówki, np.`OPTION (HASH JOIN)`
  * W programie SQL Server `ORDER BY` klauzuli, która nie jest powiązany z `TOP 100 PERCENT` w `SELECT` — klauzula

* Instrukcje SQL innych niż `SELECT` są rozpoznawane automatycznie jako zezwalają na składanie. W konsekwencji pełnych wyników procedury składowane są zawsze zwracana do klienta i wszystkie operatory LINQ zastosowane po `FromSql` są obliczane w pamięci. 

## <a name="basic-raw-sql-queries"></a>Podstawowe raw zapytania SQL

Można użyć *FromSql* — metoda rozszerzenia, aby rozpocząć zapytania LINQ na podstawie pierwotnych zapytania SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Nieprzetworzona zapytania SQL może służyć do wykonywania procedury składowanej.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Przekazywanie parametrów

Podobnie jak w przypadku jakiegokolwiek interfejsu API, który akceptuje SQL, koniecznie parametryzacja wszystkie dane wejściowe użytkownika mają ochrony przed atakiem iniekcji kodu SQL. Może zawierać parametru symbole zastępcze w ciągu zapytania SQL, a następnie podaj wartości parametrów jako dodatkowe argumenty. Należy podać wartości parametrów zostaną automatycznie przekonwertowane na `DbParameter`.

Poniższy przykład przekazuje jeden parametr do procedury składowanej. Gdy wygląda jak `String.Format` składni, podana wartość jest ujęte w where dodaje parametr i nazwę parametru wygenerowanego `{0}` symbol zastępczy został określony.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Jest to ten sam zapytania, ale przy użyciu składni interpolacji ciąg, który jest obsługiwany w programie EF Core 2.0 i nowszych wersjach:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Można również utworzyć DbParameter i podać go jako wartości parametru. Dzięki temu można używać parametrów nazwanych w ciągu zapytania SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Tworzenie za pomocą LINQ

Jeśli zapytanie SQL mogą być składane na w bazie danych, można utworzyć na podstawie początkowego raw zapytania SQL przy użyciu operatorów LINQ. Zapytania SQL, które mogą być składane z tego z `SELECT` — słowo kluczowe.

W poniższym przykładzie użyto raw zapytanie SQL, który wybiera z funkcji zwracającej tabelę (TVF), a następnie Redaguj na nim za pomocą LINQ, aby wykonać filtrowanie i sortowanie.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>W tym powiązane dane

Tworzenie przy użyciu operatorów LINQ może służyć do uwzględnienia powiązanych danych w zapytaniu.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Zawsze używaj parametryzacja raw kwerendy SQL:** interfejsów API, które akceptuje raw SQL string, takich jak `FromSql` i `ExecuteSqlCommand` zezwalają na wartości, które mają być łatwo przekazane jako parametry. Oprócz sprawdzania poprawności danych wejściowych użytkownika, należy zawsze używać parametryzacja dla każdej wartości nieprzetworzonej zapytania SQL/polecenia. Jeśli używasz ciągów dynamicznie tworzenie dowolną część ciągu zapytania, a następnie jest odpowiedzialny za sprawdzanie poprawności żadnych danych do ochrony przez atakami iniekcji kodu SQL.
