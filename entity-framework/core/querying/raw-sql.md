---
title: Pierwotne zapytania SQL - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 3024c0101c9d886ef844d1b7dc85aaf1be27e86b
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2019
ms.locfileid: "58914081"
---
# <a name="raw-sql-queries"></a>Pierwotne zapytania SQL

Entity Framework Core umożliwia lista rozwijana umożliwiająca pierwotne zapytania SQL podczas pracy z usługą relacyjnej bazy danych. Może to być przydatne, jeśli zapytanie, które należy wykonać, nie można wyrazić za pomocą LINQ lub przy użyciu zapytania LINQ wynikiem jest nieefektywne zapytania SQL. Pierwotne zapytania SQL może zwrócić typów jednostek, lub, począwszy od programu EF Core 2.1, [typy zapytań](xref:core/modeling/query-types) będących częścią modelu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="basic-raw-sql-queries"></a>Podstawowe pierwotne zapytania SQL

Możesz użyć *FromSql* metodę rozszerzenia, aby rozpocząć zapytanie LINQ, na podstawie pierwotne zapytania SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Pierwotne zapytania SQL może służyć do wykonywania procedury składowanej.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Przekazywanie parametrów

Podobnie jak w przypadku dowolnego interfejsu API, który akceptuje SQL, jest ważne, aby zdefiniować parametry wszystkie dane wejściowe użytkownika mają ochronę przed ataku polegającego na iniekcji SQL. Można zawierają symbole zastępcze parametr ciągu zapytania SQL, a następnie podaj wartości parametrów jako dodatkowe argumenty. Możesz podać wartości parametrów zostaną automatycznie przekonwertowane na `DbParameter`.

Poniższy przykład przekazuje pojedynczy parametr do procedury składowanej. Chociaż może to wyglądać jak `String.Format` składni, podana wartość jest opakowana w miejsce dodaje parametr i nazwę parametru wygenerowanego `{0}` określono symbolu zastępczego.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

To jest taka sama zapytania, ale przy użyciu składni interpolacji ciągu, który jest obsługiwany w programie EF Core 2.0 i nowszych:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Można również utworzyć DbParameter i podać go jako wartość parametru:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Dzięki temu można użyć nazwanych parametrów ciągu zapytania SQL, co jest przydatne, gdy procedura składowana ma następujące parametry opcjonalne:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Tworzenie za pomocą LINQ

Jeśli zapytanie SQL może być składana na w bazie danych, można utworzyć na podstawie początkowego pierwotne zapytania SQL przy użyciu operatorów LINQ. Zapytania SQL, które mogą być składane na zaczynają się od `SELECT` — słowo kluczowe.

W poniższym przykładzie użyto pierwotne zapytanie SQL, które wybiera z funkcji Table-Valued (TVF), a następnie komponuje się na nim za pomocą LINQ, aby wykonać filtrowanie i sortowanie.

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

Wysyła zapytanie, które używają `FromSql()` wykonaj dokładnie tę samą zmianę śledzenie reguły jako inne zapytania LINQ w wersji EF Core. Na przykład jeśli zapytanie projektów typów jednostek, wyniki będą śledzone domyślnie.  

W poniższym przykładzie użyto pierwotne zapytania SQL, wybierające z funkcji Table-Valued (TVF), a następnie wyłącza Zmień śledzenie z wywołaniem. AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>W tym powiązane dane

`Include()` Metoda może służyć do obejmują powiązanych danych, podobnie jak każde inne zapytanie LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Ograniczenia

Istnieją pewne ograniczenia, które należy zwrócić uwagę podczas korzystania z pierwotne zapytania SQL:

* Zapytanie SQL musi zwracać dane dotyczące wszystkich właściwości typu obiekt lub kwerendę.

* Nazwy kolumn w zestawie wyników muszą być zgodne nazwy kolumn, które są mapowane właściwości. Należy pamiętać, że to różni się od EF6 gdzie mapowania właściwości/kolumn została zignorowana dla pierwotne zapytania SQL i nazw musiały być zgodne z nazwami właściwości kolumny zestawu wyników.

* Zapytania SQL nie może zawierać powiązane dane. Jednak w wielu przypadkach można utworzyć na podstawie zapytania za pomocą `Include` operator, aby zwrócić dane dotyczące (zobacz [powiązanych danych w tym](#including-related-data)).

* `SELECT` Konfigurowalna powinno być zazwyczaj instrukcji przekazany do tej metody: EF Core musi ocenić operatorów dodatkowych zapytań na serwerze (na przykład, aby przetłumaczyć operatorów LINQ zastosowane po `FromSql`), SQL podane są traktowane jak podzapytania. Oznacza to, SQL przekazywane nie powinna zawierać żadnych znaków lub opcje, które nie są prawidłowe w podzapytaniu, takich jak:
  * końcowe średnikami
  * W programie SQL Server, końcowe wskazówka poziomie zapytania (na przykład `OPTION (HASH JOIN)`)
  * W programie SQL Server `ORDER BY` klauzula, która nie jest powiązany z `TOP 100 PERCENT` w `SELECT` — klauzula

* Instrukcje SQL innych niż `SELECT` są rozpoznawane automatycznie jako innego niż konfigurowalna. W konsekwencji pełnych wyników procedury składowane zawsze są zwracane do klienta i dowolnych operatorów LINQ zastosowane po `FromSql` jest oceniana w pamięci.

> [!WARNING]  
> **Zawsze używaj parametryzacji pierwotne zapytania SQL:** Oprócz sprawdzania poprawności danych wejściowych użytkownika, należy zawsze używać parametryzacji dla każdej wartości pierwotne zapytania SQL/polecenia. Interfejsy API, które akceptują pierwotne SQL string, takich jak `FromSql` i `ExecuteSqlCommand` Zezwalaj na wartości, które mają być łatwo przekazywane jako parametry. Przeciążenia `FromSql` i `ExecuteSqlCommand` akceptujących FormattableString również zezwalają na użycie syntaxt interpolacji ciągu w taki sposób, która pomaga chronić przed atakami polegającymi na iniekcji SQL. 
> 
> Jeśli są dynamicznie tworzenie dowolnej części ciągu zapytania za pomocą ciągów lub interpolacji lub przekazywanie danych wejściowych użytkownika do instrukcji lub procedur składowanych, które umożliwiają wykonanie tych danych wejściowych jako dynamiczny język SQL, jesteś odpowiedzialny za sprawdzanie poprawności wszystkie dane wejściowe Zabezpiecz się przed atakami polegającymi na iniekcji SQL.
