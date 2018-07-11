---
title: What's new in EF Core 2.0 — EF Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 538458cf49ee86b9a5cba2f606adc04e583605e2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949130"
---
# <a name="new-features-in-ef-core-20"></a>Nowe funkcje programu EF Core 2.0

## <a name="net-standard-20"></a>.NET standard 2.0
EF Core teraz jest przeznaczony dla .NET Standard 2.0, co oznacza, że można pracować przy użyciu platformy .NET Core 2.0, .NET Framework 4.6.1 i innych bibliotek, które implementują .NET Standard 2.0.
Zobacz [obsługiwane implementacje platformy .NET](../platforms/index.md) więcej informacji o tym, co jest obsługiwane.

## <a name="modeling"></a>Modelowanie

### <a name="table-splitting"></a>Dzielenie tabeli

Teraz jest możliwa do mapowania dwóch lub więcej typów jednostek do tej samej tabeli, gdzie zostaną udostępnione kolumn klucza podstawowego, a każdy wiersz odpowiada dwóch lub więcej podmiotów.

Można użyć tabeli podział identyfikującą relację (gdzie właściwości klucza obcego tworzą klucz podstawowy) musi być skonfigurowany między wszystkie typy jednostek, udostępnianie w tabeli:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Typy należące do firmy

Typ jednostki należące do firmy można udostępnić tego samego typu CLR z innym typem jednostki należące do firmy, ale ponieważ nie można zidentyfikować przez typ CLR musi istnieć nawigację do niego z innego typu jednostki. Obiekt zawierający definiujące nawigacji jest właścicielem. Podczas wykonywania zapytań dotyczących właściciela należących do typów będą uwzględniane domyślnie.

Zgodnie z Konwencją klucza podstawowego w tle zostanie utworzony dla typu należące do firmy i będzie można zamapować na tej samej tabeli jako właściciel przy użyciu dzielenie tabeli. Dzięki temu Użyj należące do typów podobnie jak złożonych typów są używane w EF6:

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
Odczyt [sekcji posiadane typy jednostek](xref:core/modeling/owned-entities) Aby uzyskać więcej informacji na temat tej funkcji.

### <a name="model-level-query-filters"></a>Filtry na poziomie modelu kwerendy

EF Core 2.0 zawiera nową funkcję nazywamy filtry kwerendy na poziomie modelu. Ta funkcja umożliwia predykatów zapytań LINQ (wyrażenia logicznego zwykle są przekazywane do operatora zapytania LINQ w przypadku gdy) należy zdefiniować bezpośrednio na typy jednostek w modelu metadanych (zwykle w OnModelCreating). Takie filtry są automatycznie stosowane do żadnych zapytań LINQ, obejmujące tych typów jednostek, w tym odwołania do właściwości nawigacji odwołanie pośrednio, takie jak przy użyciu Include lub bezpośredniego typów jednostek. Niektóre typowe aplikacje tej funkcji są następujące:

- Usuwanie nietrwałe — typy jednostek definiuje właściwość IsDeleted.
- Wielodostępność — typ jednostki definiuje właściwość identyfikatora dzierżawcy.

Poniżej przedstawiono prosty przykład Demonstrowanie funkcji w przypadku dwóch scenariuszy wymienionych powyżej:

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
Definiujemy filtr na poziomie modelu, który implementuje wielodostępu i usuwania nietrwałego dla wystąpienia elementu ```Post``` typu jednostki. Zwróć uwagę na użycie właściwości poziomu wystąpienia typu DbContext: ```TenantId```. Filtry na poziomie modelu będzie używać wartości z wystąpienia poprawny kontekst (czyli wystąpienia kontekstu wykonywanego zapytania).

Filtry mogą być wyłączone dla poszczególnych zapytań LINQ, za pomocą operatora IgnoreQueryFilters().

#### <a name="limitations"></a>Ograniczenia

- Nawigacja odwołania nie są dozwolone. Ta funkcja mogą być dodawane w oparciu o opinie.
- Filtry można zdefiniować tylko w katalogu głównym typ jednostki dla hierarchii.

### <a name="database-scalar-function-mapping"></a>Mapowanie funkcji skalarnej bazy danych

EF Core 2.0 obejmuje ważnym wkładem z [Paul Middleton](https://github.com/pmiddleton) umożliwiająca mapowania funkcji skalarnych bazy danych do metody zastępczych, dzięki czemu mogą być używane w kwerendach LINQ i przetłumaczone do bazy danych SQL.

Poniżej przedstawiono krótki opis sposobu użycia funkcji:

Zadeklaruj metodę statyczną na Twoje `DbContext` i Adnotuj ją za pomocą `DbFunctionAttribute`:

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

Metod, takich jak to są automatycznie rejestrowane. Po zarejestrowaniu wywołania metody w zapytaniu programu LINQ mogą być tłumaczone do wywołania funkcji w języku SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Kilka kwestii, które należy zwrócić uwagę:

- Zgodnie z Konwencją Nazwa metody jest używana jako nazwa funkcji (w tym przypadku funkcję zdefiniowaną przez użytkownika) podczas generowania SQL, ale można zastąpić nazwę i schematu podczas rejestracji — metoda
- Obecnie obsługiwane są tylko funkcje skalarne
- Należy utworzyć zamapowanego funkcji w bazie danych. EF Core migracje nie zajmie się jego tworzenia

### <a name="self-contained-type-configuration-for-code-first"></a>Konfiguracja typu niezależna kodu pierwszy

W EF6 było możliwe do hermetyzacji kodu konfiguracji pierwszego określonego typu, wynikające z *EntityTypeConfiguration*. W programie EF Core 2.0 możemy ponownie także tego wzorca:

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

Podstawowy wzorzec przy użyciu programu EF Core w aplikacji ASP.NET Core zwykle obejmuje rejestrowania niestandardowego typu DbContext w systemie iniekcji zależności i później uzyskanie wystąpień tego typu za pomocą konstruktora parametrów w kontrolerów. Oznacza to, że nowe wystąpienie klasy kontekstu DbContext jest tworzony dla każdego żądania.

W wersji 2.0 wprowadzamy nowy sposób rejestrowania niestandardowego typu DbContext w wstrzykiwanie zależności, które wprowadza w sposób niewidoczny dla użytkownika pulę wystąpień wielokrotnego użytku DbContext. Aby użyć typu DbContext buforowanie, użyj ```AddDbContextPool``` zamiast ```AddDbContext``` podczas rejestracji usługi:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Jeśli ta metoda jest używana, w momencie wystąpienia typu DbContext jest wymagany przez kontrolera, że firma Microsoft będzie najpierw sprawdź, czy wystąpienie dostępne w puli. Po Kończenie znajdujących się przetwarzanie żądań, każdy stan w wystąpieniu jest resetowany, a wystąpienie jest zwracane do puli.

Jest to zachowuje się podobnie jak jak buforowanie połączeń działa w dostawcy ADO.NET i ma tę zaletę zapisywania koszt inicjowania wystąpienia typu DbContext.

### <a name="limitations"></a>Ograniczenia

Nowa metoda wprowadza pewne ograniczenia na co można zrobić ```OnConfiguring()``` metoda kontekstu DbContext.

> [!WARNING]  
> Należy unikać używania buforowania DbContext, jeśli to Ty masz własne stanie (na przykład pola prywatne) w swojej otrzymanej klasy DbContext, który nie powinien być współużytkowane przez wiele żądań. EF Core tylko spowoduje zresetowanie stanu który zna przed dodaniem wystąpienia typu DbContext do puli.

### <a name="explicitly-compiled-queries"></a>Zapytania skompilowane jawnie

Jest to drugi zdecydować się na wydajności funkcji oferują korzyści w scenariuszach o dużej skali.

Ręczne lub jawnie skompilowanych zapytanie interfejsy API zostały dostępne w poprzednich wersjach programu EF, a także w składniku LINQ to SQL, aby umożliwić aplikacjom tak, aby może zostać obliczony tylko raz w pamięci podręcznej tłumaczenie zapytań i wykonywane wiele razy.

Chociaż ogólnie rzecz biorąc programu EF Core można automatycznie kompilują i kwerendy oparte na mieszanych reprezentacja wyrażenia zapytania w pamięci podręcznej, mechanizm ten może służyć do uzyskania małych są bardziej wydajne, pomijając obliczania skrótu i przeszukiwania pamięci podręcznej, dzięki czemu aplikacji do korzystania z zapytania już skompilowane, za pośrednictwem wywołania delegata.

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

EF Core obsługuje automatyczne generowanie wartości klucza przy użyciu różnych mechanizmów. Podczas używania tej funkcji, wartość jest generowany, gdy właściwość klucza jest ustawieniem domyślnym CLR — zwykle zero lub wartość null. Oznacza to, że wykres jednostki mogą być przekazywane do `DbContext.Attach` lub `DbSet.Attach` i programem EF Core spowoduje oznaczenie tych obiektów, które mają klucz, który został już ustawiony jako `Unchanged` podczas tych jednostek, które nie mają zestawu kluczy zostaną oznaczone jako `Added`. Ułatwia można dołączyć wykres mieszane nowych i istniejących jednostek, korzystając z wygenerowanych kluczy. `DbContext.Update` i `DbSet.Update` działają w taki sam sposób, z tą różnicą, że jednostki z zestawu kluczy, są oznaczone jako `Modified` zamiast `Unchanged`.

## <a name="query"></a>Zapytanie

### <a name="improved-linq-translation"></a>Ulepszone LINQ tłumaczenia

Umożliwia więcej zapytań pomyślnie wykonać logiką więcej ocenianego w bazie danych (a nie w pamięci), a dane niepotrzebnie pobrane z bazy danych.

### <a name="groupjoin-improvements"></a>Groupjoin — ulepszenia

Ta Praca zwiększa SQL Server, który jest generowany dla sprzężenia grupowane. Sprzężenia grupowane w większości przypadków są wynikiem podzapytaniami właściwości nawigacji opcjonalne.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolacja ciągów w FromSql i ExecuteSqlCommand

C# 6 wprowadzono Interpolacja ciągów funkcja, która umożliwia wyrażeń języka C# do osadzenia bezpośrednio Literały ciągu, dzięki czemu ładny building ciągów w czasie wykonywania. W programie EF Core 2.0 dodaliśmy specjalne obsługę ciągów interpolowanych do naszych dwa podstawowe interfejsów API, które akceptują pierwotne ciągów SQL: ```FromSql``` i ```ExecuteSqlCommand```. Ta obsługa nowych umożliwia interpolacji ciągu C# ma być używany w sposób "bezpieczne". Oznacza to w sposób zapewniający ochronę przed typowych pomyłek iniekcji SQL, które mogą wystąpić podczas dynamicznego tworzenia SQL w czasie wykonywania.

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

W tym przykładzie istnieją dwie zmienne osadzone w ciągu formatu SQL. EF Core generuje następujące instrukcje SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

Dodaliśmy EF. Właściwości funkcji, która może być używany przez programu EF Core lub dostawców do definiowania metod mapowane na funkcje bazy danych lub operatorów, tak, aby te mogą być wywoływane w zapytaniach LINQ. Pierwszy przykład taka metoda jest Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Należy pamiętać, że Like() pochodzi z implementacją w pamięci, które mogą być przydatne podczas pracy w bazie danych w pamięci lub oceny predykatu musi nastąpić po stronie klienta.

## <a name="database-management"></a>Zarządzanie bazą danych

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Pluralizacja podłączania do tworzenia szkieletów DbContext

EF Core 2.0 wprowadzono nowy *IPluralizer* usługa, która jest używana do końcówek jednostki wpisz nazwy użytkowników i DbSet nazwy w liczbie mnogiej. Domyślna implementacja jest pusta, więc jest to po prostu podłączania, gdzie osoby zajmujące się łatwo podłączyć w ich własnych pluralizer.

Poniżej przedstawiono, jak to wygląda dla dewelopera, można dołączyć w ich własnych pluralizer:

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
To daje nam bardziej niezawodne rozwiązanie w Microsoft.Data.Sqlite dystrybucji natywnych SQLite plików binarnych na różnych platformach.

### <a name="only-one-provider-per-model"></a>Tylko jeden dostawca na modelu
Znacznie rozszerzają sposobu interakcji z modelem dostawców i upraszcza sposób działania Konwencji, adnotacji i płynnych interfejsów API za pomocą różnych dostawców.

EF Core 2.0 będą teraz tworzyć inną [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) dla każdego innego dostawcy używane. Jest to zazwyczaj niewidoczna dla aplikacji. Ma to ułatwione uproszczenia metadanych niskiego poziomu interfejsy API, taki sposób, że dowolny dostęp do *typowe pojęcia relacyjnych metadanych* zawsze jest wykonywane za pomocą wywołania `.Relational` zamiast `.SqlServer`, `.Sqlite`itp.

### <a name="consolidated-logging-and-diagnostics"></a>Skonsolidowane rejestrowania i diagnostyki

Rejestrowanie (oparte na ILogger) i mechanizmy diagnostyki (oparte na DiagnosticSource) teraz udostępnić więcej kodu.

Identyfikatory zdarzeń dla wiadomości wysyłanych do ILogger zostały zmienione w wersji 2.0. Identyfikatory zdarzeń obecnie są unikatowe w obrębie kod programem EF Core. Te komunikaty teraz również wykonać standardowego wzorca dla rejestrowaniem strukturalnym używane przez, na przykład MVC.

Kategorie rejestratora również zostały zmienione. Ma teraz dobrze znanego zestawu kategorii dostępne za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Zdarzenia DiagnosticSource teraz użyć takich samych nazwach identyfikator zdarzenia odpowiadającego `ILogger` wiadomości.
