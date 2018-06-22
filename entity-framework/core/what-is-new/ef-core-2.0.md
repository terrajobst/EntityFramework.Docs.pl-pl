---
title: Co to jest nowa w programie EF 2.0 Core - EF Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 02d0b6fe2956e819e08e08c9a0658008abd36c34
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29680163"
---
# <a name="new-features-in-ef-core-20"></a>Nowe funkcje w programie EF Core 2.0

## <a name="net-standard-20"></a>.NET Standard 2.0
Podstawowe EF dotyczy teraz .NET Standard 2.0, co oznacza, że może współpracować z .NET Core 2.0, .NET Framework 4.6.1 i innych bibliotek, które implementuje standardowe .NET w wersji 2.0.
Zobacz [obsługiwane implementacje .NET](../platforms/index.md) uzyskać więcej informacji dotyczących co jest obsługiwane.

## <a name="modeling"></a>Modelowanie

### <a name="table-splitting"></a>Dzielenie tabeli

Obecnie istnieje możliwość zamapować co najmniej dwa typy jednostek do tej samej tabeli, w którym będą udostępniane z kolumn klucza podstawowego i każdy wiersz odpowiada dwóch lub więcej jednostek.

Aby tabeli dzielenia relacji identyfikującej (w którym właściwości klucza obcego tworzą klucza podstawowego) musi być skonfigurowany między wszystkie typy jednostek, udostępnianie w tabeli:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Typy należące do firmy

Typ jednostki należące do firmy mogą udostępniać tego samego typu CLR z innym typem należących do jednostki, ale ponieważ nie można zidentyfikować przez typ CLR musi istnieć nawigację do niego z innego typu jednostki. Obiekt zawierający definiującego nawigacji jest właścicielem. Podczas wykonywania zapytania właściciela należących do typów zostaną uwzględnione domyślnie.

Konwencja klucz podstawowy w tle zostanie utworzona dla typu należące do firmy i będzie on zamapowany do tej samej tabeli jako właściciel przy użyciu dzielenia tabeli. Dzięki temu Użyj należące do typów podobnie jak złożonych typów są używane w EF6:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```
Odczyt [sekcję na temat typów jednostek będących własnością](xref:core/modeling/owned-entities) Aby uzyskać więcej informacji na temat tej funkcji.

### <a name="model-level-query-filters"></a>Filtry kwerendy na poziomie modelu

Podstawowe EF 2.0 zawiera nową funkcję nazywamy filtrów kwerendy na poziomie modelu. Ta funkcja umożliwia predykaty zapytań LINQ (wyrażenia logicznego zwykle przekazane do operatora zapytania LINQ gdzie) należy zdefiniować bezpośrednio na typy jednostek w modelu metadanych (zwykle w OnModelCreating). Takie filtry są automatycznie stosowane do wszelkich zapytań LINQ tych typów jednostek, w tym odwołań do właściwości nawigacji do którego istnieje odwołanie pośrednie, takie jak przy użyciu Include lub bezpośredniego typów jednostek. Niektóre typowe zastosowania tej funkcji są następujące:

- Usuń soft - typów jednostek definiuje właściwość IsDeleted.
- Wielodostępność — typ jednostki definiuje właściwość identyfikatora dzierżawcy.

Poniżej przedstawiono prosty przykład prezentacja funkcji w dwóch scenariuszach wymienione powyżej:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
Definicję filtru na poziomie modelu, który implementuje obsługi wielu dzierżawców i soft usunięcie wystąpienia ```Post``` typu jednostki. Zwróć uwagę na użycie właściwości poziomu wystąpienia typu DbContext: ```TenantId```. Filtry na poziomie modelu użyje wartości z wystąpienia poprawny kontekst. Tj. ten, który wykonuje zapytania.

Filtry mogą być wyłączone dla poszczególnych zapytań LINQ za pomocą operatora IgnoreQueryFilters().

#### <a name="limitations"></a>Ograniczenia

- Odwołania nawigacji nie są dozwolone. Ta funkcja może być dodany oparte na opinii.
- Filtry można zdefiniować tylko w katalogu głównym typu jednostki z hierarchii.

### <a name="database-scalar-function-mapping"></a>Mapowanie funkcji skalarnych bazy danych

Podstawowe EF 2.0 zawiera ważne wkładu [Middleton Pawła](https://github.com/pmiddleton) co pozwala mapowania funkcji skalarnych bazy danych do metody zastępcze, dzięki czemu mogą być używane w zapytaniach LINQ i przetłumaczyć na język SQL.

Poniżej przedstawiono krótki opis sposobu użycia funkcji:

Deklaruje metody statycznej na Twojej `DbContext` i adnotacje go przy użyciu `DbFunctionAttribute`:

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

Metody, takie jak to automatycznie są rejestrowane. Po zarejestrowaniu wywołania metody w zapytaniu składnika LINQ mogą być przekonwertowana na wywołania funkcji w programie SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Kilka rzeczy do uwzględnienia:

- Konwencja Nazwa metody jest używana jako nazwa funkcji (w tym przypadku funkcja zdefiniowana przez użytkownika) podczas generowania SQL, ale można zastąpić nazwę i schematu podczas rejestracji — metoda
- Obecnie obsługiwane są tylko funkcje skalarne
- Funkcja mapowane należy utworzyć w bazie danych, np. Core EF migracji nie zajmie się jej tworzenia

### <a name="self-contained-type-configuration-for-code-first"></a>Konfiguracja niezależne typu dla kodu pierwszy

W EF6 było możliwe Hermetyzowanie kod pierwszego konfiguracji określonego typu przez wynikających z *typu EntityTypeConfiguration*. W programie EF Core 2.0 możemy są dostarczają tego wzorca ponownie:

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a>Wysoka wydajność

### <a name="dbcontext-pooling"></a>Buforowanie typu DbContext

Podstawowy wzorzec używania EF Core w aplikacji platformy ASP.NET Core zwykle obejmuje rejestrowania niestandardowego typu DbContext w systemie iniekcji zależności i później uzyskiwania wystąpień tego typu za pomocą parametrów konstruktora kontrolery. Oznacza to, że nowe wystąpienie klasy DbContext jest tworzony dla każdego żądania.

W wersji 2.0 wprowadzamy nowy sposób rejestrowania niestandardowego typu DbContext w iniekcji zależności, które prezentuje sposób niewidoczny dla użytkownika puli wielokrotnego użytku wystąpienia typu DbContext. Aby korzystać z pul DbContext, użyj ```AddDbContextPool``` zamiast ```AddDbContext``` podczas rejestracji usługi:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Jeśli ta metoda jest używana, w tym czasie wystąpienie typu DbContext jest wymagany przez kontrolera się, że firma Microsoft będzie najpierw sprawdź, czy wystąpienie dostępne w puli. Po Kończenie znajdujących się przetwarzanie żądania, każdy stan w wystąpieniu jest resetowany, a wystąpienie jest zwrócony do puli.

To jest podobny do sposobu użycia puli połączeń działa w dostawcy ADO.NET i daje możliwość zapisywania koszt inicjowania wystąpienie typu DbContext.

### <a name="limitations"></a>Ograniczenia

Nowa metoda wprowadzono kilka ograniczenia co można zrobić ```OnConfiguring()``` metody DbContext.

> [!WARNING]  
> Unikaj przy użyciu buforowania DbContext, jeśli obsługa własne stanu (np. pola prywatne) w klasie pochodnej DbContext, który nie powinien być współużytkowane przez wiele żądań. Podstawowe EF tylko spowoduje zresetowanie stanu, który zna przed dodaniem wystąpienie typu DbContext do puli.

### <a name="explicitly-compiled-queries"></a>Jawnie skompilowane zapytania

Jest to drugi zdecydować się na wydajności funkcje oferują korzyści w scenariuszach wysokiej skali.

Ręczne lub jawnie skompilowanego zapytania zostały dostępne w poprzednich wersjach programu EF, a także w składniku LINQ to SQL, aby umożliwić aplikacjom pamięci podręcznej w tłumaczeniu wartości zapytania, aby tylko jeden raz obliczana interfejsów API i wykonywane wiele razy.

Ogólnie rzecz biorąc automatycznie EF Core skompilować i pamięci podręcznej zapytania oparte na mieszanych reprezentacja wyrażenia zapytania, ten mechanizm może służyć do uzyskania bardziej wydajne małych pomijając obliczania skrótu i wyszukiwania w pamięci podręcznej, dzięki czemu aplikacji do korzystania z już skompilowane zapytania za pomocą wywołania delegata.

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a>Śledzenie zmian

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Dołącz można śledzić wykres nowych i istniejących jednostek

Jądro EF obsługuje automatyczne generowanie wartości klucza przy użyciu różnych mechanizmów. Podczas używania tej funkcji, wartość jest generowany, gdy właściwość klucza jest domyślnym CLR — zwykle zero lub wartość null. Oznacza to, że wykres jednostek mogą zostać przekazane do `DbContext.Attach` lub `DbSet.Attach` i EF Core spowoduje oznaczenie tych obiektów, które mają klucz, który został już ustawiony jako `Unchanged` podczas tych jednostek, które nie mają ustawionego klucza zostaną oznaczone jako `Added`. Ułatwia dołączyć wykres mieszany nowych i istniejących obiektów przy użyciu generowane kluczy. `DbContext.Update` i `DbSet.Update` działa w taki sam sposób, z tą różnicą, że jednostki z zestawu kluczy są oznaczone jako `Modified` zamiast `Unchanged`.

## <a name="query"></a>Zapytanie

### <a name="improved-linq-translation"></a>Tłumaczenie ulepszone LINQ

Włącza więcej zapytania, aby pomyślnie wykonać z logiką więcej oceniane w bazie danych (a nie w pamięci) i mniejsza ilość danych niepotrzebnie pobrane z bazy danych.

### <a name="groupjoin-improvements"></a>Ulepszenia GroupJoin

Praca ta poprawia SQL Server, który jest generowany dla grupy sprzężeń. Sprzężenia grupowane najczęściej są wynikiem podzapytaniach dla właściwości nawigacji opcjonalne.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Ciąg interpolacji FromSql i ExecuteSqlCommand

C# 6 wprowadzony ciąg interpolacji, funkcja, która umożliwia wyrażeń C# bezpośrednio osadzone w literałach ciągu, dzięki czemu nieuprzywilejowany budynku ciągów w czasie wykonywania. W programie EF Core 2.0 dodaliśmy specjalne obsługę ciągi interpolowane do naszych dwa podstawowe interfejsów API akceptujących raw ciągów SQL: ```FromSql``` i ```ExecuteSqlCommand```. Ta nowa funkcja obsługi umożliwia C# interpolacji ciąg do użycia w sposób "bezpiecznym". Np. w sposób zapewniający ochronę przed typowych pomyłek iniekcji kodu SQL, które mogą wystąpić podczas dynamiczne budowanie SQL w czasie wykonywania.

Oto przykład:

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

W tym przykładzie istnieją dwie zmienne osadzone w formacie ciągu SQL. Podstawowe EF utworzy następujące instrukcje SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

Dodaliśmy EF. Właściwość funkcji używany przez podstawowych funkcji EF lub dostawców w celu zdefiniowania metody, które mapują funkcje bazy danych i operatorom tak, aby te mogą być wywoływane w zapytaniach składnika LINQ. Pierwszym przykładzie taka metoda jest Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

Należy pamiętać, że Like() pochodzi z implementacją w pamięci, które mogą być przydatne podczas pracy w bazie danych w pamięci lub gdy oceny predykatu musi występować po stronie klienta.

## <a name="database-management"></a>Zarządzanie bazą danych

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Hak określania liczby mnogiej dla szkieletów DbContext

Podstawowe EF 2.0 wprowadzono nowy *IPluralizer* , który służy do nadaj jednostki wpisz nazwy i DbSet nazwy w liczbie mnogiej. Domyślna implementacja jest zerowa, więc jest po prostu haku, gdzie pracowników, można łatwo podłączyć własnych pluralizer.

Oto, co wygląda dla deweloperów na utworzenie punktu zaczepienia w ich własnych pluralizer:

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a>Inne osoby

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Przenieś dostawcy ADO.NET SQLite SQLitePCL.raw
Zapewnia nam bardziej niezawodne rozwiązanie Microsoft.Data.Sqlite dystrybucji natywne SQLite pliki binarne na różnych platformach.

### <a name="only-one-provider-per-model"></a>Tylko jednego dostawcę dla modelu
Znacznie rozszerza dostawców można interakcję z modelu i upraszcza, jak działają adnotacje, fluent API i konwencje z różnych dostawców.

Podstawowe EF 2.0 teraz kompilacji w innej [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) dla każdego używanego innego dostawcy. Jest to zazwyczaj niewidoczne dla aplikacji. Ma to ułatwić uproszczenia metadanych niższego poziomu interfejsów API tak, aby wszelkie dostęp do *wspólne pojęcia relacyjne metadanych* jest zawsze wykonywane za pośrednictwem wywołania `.Relational` zamiast `.SqlServer`, `.Sqlite`itp.

### <a name="consolidated-logging-and-diagnostics"></a>Skonsolidowane rejestrowania i diagnostyki

Rejestrowanie (oparte na ILogger) i mechanizmów diagnostyki (oparte na DiagnosticSource) teraz udostępniać więcej kodu.

Identyfikatory zdarzeń komunikatów wysyłanych do ILogger zostały zmienione w 2.0. Identyfikatory zdarzeń obecnie są unikatowe w EF rdzeń kodu. Te komunikaty teraz również wykonać standardowego wzorca strukturalnych rejestrowania używany przez, na przykład MVC.

Kategorie rejestratora również zostały zmienione. Ma teraz dobrze znanego zestawu kategorii dostępne za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Zdarzenia DiagnosticSource teraz użyć tej samej nazwy Identyfikatora zdarzenia odpowiadającego `ILogger` wiadomości.
