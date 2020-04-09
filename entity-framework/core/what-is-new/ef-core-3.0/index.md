---
title: Nowe funkcje w entity framework core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebc676930ffc396aa70bb8afb91cf5a0cd43e04d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417943"
---
# <a name="new-features-in-entity-framework-core-30"></a>Nowe funkcje w entity framework core 3.0

Poniższa lista zawiera główne nowe funkcje w EF Core 3.0.

Jako główne wydanie EF Core 3.0 zawiera również kilka [istotnych zmian,](xref:core/what-is-new/ef-core-3.0/breaking-changes)które są ulepszenia interfejsu API, które mogą mieć negatywny wpływ na istniejące aplikacje.  

## <a name="linq-overhaul"></a>Remont LINQ

LINQ umożliwia pisanie zapytań bazy danych przy użyciu wybranego języka .NET, korzystając z informacji o typie rozszerzonym, aby oferować intellisense i sprawdzanie typu kompilacji.
Ale LINQ umożliwia również pisanie nieograniczonej liczby skomplikowanych zapytań zawierających dowolne wyrażenia (wywołania metody lub operacje).
Jak obsługiwać wszystkie te kombinacje jest głównym wyzwaniem dla dostawców LINQ.

W EF Core 3.0 firma Microsoft rearchitected naszego dostawcy LINQ, aby umożliwić tłumaczenie więcej wzorców zapytań do SQL, generowanie efektywnych zapytań w większej liczbie przypadków i zapobieganie nieefektywne zapytania nie zostaniewykryte. Nowy dostawca LINQ jest podstawą, nad którą będziemy mogli zaoferować nowe możliwości zapytań i ulepszenia wydajności w przyszłych wersjach, bez przerywania istniejących aplikacji i dostawców danych.

### <a name="restricted-client-evaluation"></a>Ograniczona ocena klienta

Najważniejsza zmiana projektu ma do czynienia z tym, jak obsługujemy wyrażenia LINQ, które nie mogą być konwertowane na parametry lub przetłumaczone na SQL.

W poprzednich wersjach EF Core zidentyfikowano, jakie części kwerendy można przetłumaczyć na język SQL, a resztę zapytania wykonał na kliencie.
Ten typ wykonywania po stronie klienta jest pożądane w niektórych sytuacjach, ale w wielu innych przypadkach może spowodować nieefektywne zapytania.

Na przykład jeśli EF Core 2.2 nie może przetłumaczyć predykatu w `Where()` wywołaniu, wykonał instrukcję SQL bez filtru, przesłał wszystkie wiersze z bazy danych, a następnie odfiltrował je w pamięci:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

Może to być dopuszczalne, jeśli baza danych zawiera niewielką liczbę wierszy, ale może spowodować znaczne problemy z wydajnością lub nawet niepowodzenie aplikacji, jeśli baza danych zawiera dużą liczbę wierszy.

W EF Core 3.0 ograniczyliśmy ocenę klienta tylko do projekcji najwyższego poziomu `Select()`(zasadniczo ostatnie wezwanie).
Gdy EF Core 3.0 wykrywa wyrażenia, które nie mogą być tłumaczone w innym miejscu w kwerendzie, zgłasza wyjątek środowiska uruchomieniowego.

Aby ocenić warunek predykatu na kliencie, jak w poprzednim przykładzie, deweloperzy muszą teraz jawnie przełączyć ocenę kwerendy do LINQ do obiektów:

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

Zobacz [dokumentację zmian podziału,](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) aby uzyskać więcej informacji na temat tego, jak może to wpłynąć na istniejące aplikacje.

### <a name="single-sql-statement-per-linq-query"></a>Pojedyncza instrukcja SQL na kwerendę LINQ

Innym aspektem projektu, który zmienił się znacząco w 3.0 jest to, że teraz zawsze generuje jedną instrukcję SQL dla zapytania LINQ. W poprzednich wersjach użyliśmy do generowania wielu `Include()` instrukcji SQL w niektórych przypadkach, przetłumaczone wywołania właściwości nawigacji kolekcji i przetłumaczone zapytania, które następnie niektóre wzorce z podksy. Chociaż w niektórych przypadkach było `Include()` to wygodne, a nawet pomogło uniknąć wysyłania nadmiarowych danych przez sieć, implementacja była złożona i spowodowało pewne skrajnie nieefektywne zachowania (zapytania N +1). Były sytuacje, w których dane zwracane w wielu kwerendach był potencjalnie niespójne.

Podobnie jak w przypadku oceny klienta, jeśli EF Core 3.0 nie można przetłumaczyć zapytania LINQ na jedną instrukcję SQL, zgłasza wyjątek środowiska uruchomieniowego. Ale zrobiliśmy EF Core stanie tłumaczyć wiele typowych wzorców, które były używane do generowania wielu zapytań do pojedynczej kwerendy z jonów.

## <a name="cosmos-db-support"></a>Pomoc techniczna aplikacji Cosmos DB

Dostawca usługi Cosmos DB dla ef core umożliwia deweloperom zaznajomionym z modelem programowania EF łatwo kierować usługi Azure Cosmos DB jako bazy danych aplikacji. Celem jest, aby niektóre z zalet usługi Cosmos DB, takich jak dystrybucja globalna, "zawsze włączony" dostępność, elastyczna skalowalność i małe opóźnienia, jeszcze bardziej dostępne dla deweloperów platformy .NET. Dostawca włącza większość funkcji EF Core, takich jak automatyczne śledzenie zmian, LINQ i konwersje wartości, względem interfejsu API SQL w usłudze Cosmos DB.

Aby uzyskać więcej informacji, zobacz [dokumentację dostawcy usługi Cosmos DB.](xref:core/providers/cosmos/index)

## <a name="c-80-support"></a>Pomoc techniczna języka C# 8.0

EF Core 3.0 korzysta z kilku [nowych funkcji w języku C# 8.0:](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8)

### <a name="asynchronous-streams"></a>Strumienie asynchroniczne

Asynchroniczne wyniki kwerendy są teraz `IAsyncEnumerable<T>` udostępniane przy użyciu `await foreach`nowego standardowego interfejsu i mogą być używane przy użyciu programu .

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

Zobacz [strumieni asynchronicznych w dokumentacji języka C#,](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) aby uzyskać więcej informacji.

### <a name="nullable-reference-types"></a>Typy referencyjne dopuszczające wartość null

Gdy ta nowa funkcja jest włączona w kodzie, EF Core sprawdza nullability właściwości typu odwołania i stosuje go do odpowiednich kolumn i relacji w bazie danych: `[Required]` właściwości typów odwołań niepodważalnych są traktowane tak, jakby miały atrybut adnotacji danych.

Na przykład w następującej klasie właściwości oznaczone `string?` jako typu zostaną skonfigurowane `string` jako opcjonalne, podczas gdy będą skonfigurowane zgodnie z wymaganiami:

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

Zobacz [Praca z typami odwołań nullable](xref:core/miscellaneous/nullable-reference-types) w dokumentacji EF Core, aby uzyskać więcej informacji.

## <a name="interception-of-database-operations"></a>Przechwytywanie operacji bazy danych

Nowy interfejs API przechwytywania w EF Core 3.0 umożliwia zapewnienie niestandardowej logiki do wywoływania automatycznie, gdy operacje niskiego poziomu bazy danych występują w ramach normalnego działania EF Core. Na przykład podczas otwierania połączeń, zatwierdzania transakcji lub wykonywania poleceń.

Podobnie jak funkcje przechwytywania, które istniały w EF 6, interceptory umożliwiają przechwytywanie operacji przed lub po ich wystąpieniu. Po przechwyceniu ich, zanim się zdarzyć, można by-pass wykonanie i dostarczyć alternatywne wyniki z logiki przechwytywania.

Na przykład, aby manipulować tekstem `IDbCommandInterceptor`polecenia, można utworzyć :

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

I zarejestruj go `DbContext`w swoim:

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>Inżynieria odwrotna widoków bazy danych

Nazwy typów kwerend, które reprezentują dane, które mogą być odczytywane z bazy danych, ale nie są aktualizowane, zostały zmienione na [typy jednostek bezkluczeń](xref:core/modeling/keyless-entity-types).
Ponieważ są one doskonałe dopasowanie do mapowania widoków bazy danych w większości scenariuszy, EF Core teraz automatycznie tworzy typy jednostek bezkluzyfowych, gdy widoki bazy danych inżynierii odwrotnej.

Na przykład za pomocą [narzędzia wiersza polecenia dotnet ef](xref:core/miscellaneous/cli/dotnet) można wpisać:

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

A narzędzie będzie teraz automatycznie szkielety typów widoków i tabel bez klawiszy:

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Jednostki zależne dzielące tabelę z podmiotem głównym są teraz opcjonalne

Począwszy od EF Core 3.0, `OrderDetails` jeśli jest własnością `Order` lub jawnie mapowane do `Order` tej `OrderDetails` samej `OrderDetails` tabeli, będzie można dodać bez i wszystkie właściwości, z wyjątkiem klucza podstawowego będą mapowane do kolumn nullable.

Podczas wykonywania zapytań EF `OrderDetails` `null` Core ustawi się, jeśli którakolwiek z jego wymaganych właściwości nie ma wartości lub jeśli nie `null`ma wymaganych właściwości oprócz klucza podstawowego i wszystkie właściwości są .

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 na .NET Core

Nie jest to funkcja EF Core 3.0, ale uważamy, że jest to ważne dla wielu naszych obecnych klientów.

Rozumiemy, że wiele istniejących aplikacji używa poprzednich wersji ef i że przenoszenie ich do EF Core tylko w celu skorzystania z .NET Core może wymagać znacznego wysiłku.
Z tego powodu zdecydowaliśmy się przenieść najnowszą wersję EF 6 do uruchomienia na .NET Core 3.0.

Aby uzyskać więcej informacji, [zobacz, co nowego w EF 6](xref:ef6/what-is-new/index).

## <a name="postponed-features"></a>Przełożone funkcje

Niektóre funkcje pierwotnie planowane dla EF Core 3.0 zostały przełożone na przyszłe wersje:

- Możliwość ignorowania części modelu w migracjach, śledzonych jako [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Jednostki worka właściwości, śledzone jako dwa oddzielne problemy: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) dotyczące jednostek typu udostępnionego i [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) na temat obsługi mapowania właściwości indeksowanych.
