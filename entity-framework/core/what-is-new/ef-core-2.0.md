---
title: Co nowego w EF Core 2.0 - EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 83f6b819409d502dba17a678d44a0746a4a77f4b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417497"
---
# <a name="new-features-in-ef-core-20"></a>Nowe funkcje w EF Core 2.0

## <a name="net-standard-20"></a>.NET Standard 2.0

EF Core jest teraz przeznaczony dla platformy .NET Standard 2.0, co oznacza, że może pracować z programem .NET Core 2.0, .NET Framework 4.6.1 i innymi bibliotekami implementuuuuujymis.
Zobacz [obsługiwane implementacje platformy .NET,](../platforms/index.md) aby uzyskać więcej informacji na temat obsługiwanych.

## <a name="modeling"></a>Modelowanie

### <a name="table-splitting"></a>Dzielenie tabeli

Teraz możliwe jest mapowanie dwóch lub więcej typów jednostek do tej samej tabeli, w której będą współużytkowane kolumny klucza podstawowego, a każdy wiersz będzie odpowiadał dwóm lub więcej jednostkom.

Aby użyć podziału tabeli, relacja identyfikująca (w której właściwości klucza obcego tworzą klucz podstawowy) musi być skonfigurowana między wszystkimi typami jednostek współużytkujących tabelę:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Przeczytaj [sekcję podziału tabeli, aby](xref:core/modeling/table-splitting) uzyskać więcej informacji na temat tej funkcji.

### <a name="owned-types"></a>Typy własnością

Typ jednostki należącej do jednostki może współużytkować ten sam typ platformy .NET z innym typem jednostki należącej do niej, ale ponieważ nie można go zidentyfikować tylko przez typ .NET, musi istnieć nawigacja do niego z innego typu jednostki. Jednostki zawierające nawigacji definiującej jest właścicielem. Podczas wykonywania zapytań właściciela typy należące do właścicieli zostaną domyślnie uwzględnione.

Zgodnie z konwencją klucz podstawowy w tle zostanie utworzony dla typu należącego do właściciela i zostanie odwzorowany na tej samej tabeli co właściciel przy użyciu podziału tabeli. Dzięki temu można używać typów posiadanych podobnie jak typy złożone są używane w EF6:

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

Przeczytaj [sekcję dotyczącą typów jednostek należących](xref:core/modeling/owned-entities) do firmy, aby uzyskać więcej informacji na temat tej funkcji.

### <a name="model-level-query-filters"></a>Filtry zapytań na poziomie modelu

EF Core 2.0 zawiera nową funkcję, którą nazywamy filtrami zapytań na poziomie modelu. Ta funkcja umożliwia predykaty zapytania LINQ (wyrażenie logiczne zazwyczaj przekazywane do linq, gdzie operator kwerendy) mają być zdefiniowane bezpośrednio na typy jednostek w modelu metadanych (zwykle w OnModelCreating). Takie filtry są automatycznie stosowane do wszystkich zapytań LINQ dotyczących tych typów jednostek, w tym typy jednostek, do których odwołuje się pośrednio, na przykład za pomocą funkcji Uwzględnij lub bezpośrednie odwołania do właściwości nawigacji. Niektóre typowe zastosowania tej funkcji to:

- Usuwanie nietrwałe — typy jednostek definiuje właściwość IsDeleted.
- Multi-dzierżawy — typ jednostki definiuje TenantId właściwości.

Oto prosty przykład demonstrujący funkcję dla dwóch scenariuszy wymienionych powyżej:

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
            && p.TenantId == this.TenantId);
    }
}
```

Definiujemy filtr na poziomie modelu, który implementuje multi-dzierżawy i `Post` soft-delete dla wystąpień typu jednostki. Zwróć uwagę na `DbContext` użycie właściwości `TenantId`na poziomie wystąpienia: . Filtry na poziomie modelu użyją wartości z poprawnego wystąpienia kontekstu (czyli wystąpienia kontekstu, które wykonuje kwerendę).

Filtry mogą być wyłączone dla poszczególnych zapytań LINQ przy użyciu ignoreQueryFilters() operatora.

#### <a name="limitations"></a>Ograniczenia

- Odwołania do nawigacji nie są dozwolone. Ta funkcja może zostać dodana na podstawie opinii.
- Filtry można zdefiniować tylko w głównym typie jednostki hierarchii.

### <a name="database-scalar-function-mapping"></a>Mapowanie funkcji skalarnych bazy danych

EF Core 2.0 zawiera ważny wkład [Paul Middleton,](https://github.com/pmiddleton) który umożliwia mapowanie funkcji skalarnych bazy danych do wycinków metody, dzięki czemu mogą być używane w zapytaniach LINQ i przetłumaczone na język SQL.

Oto krótki opis sposobu użycia tej funkcji:

Zadeklarować metodę statyczną `DbContext` na swoim i `DbFunctionAttribute`opisać ją za pomocą:

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new NotImplementedException();
    }
}
```

Metody takie jak ten są automatycznie rejestrowane. Po zarejestrowaniu wywołania metody w kwerendzie LINQ można przetłumaczyć na wywołania funkcji w języku SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Kilka kwestii, na które warto zwrócić uwagę:

- Zgodnie z konwencją nazwa metody jest używana jako nazwa funkcji (w tym przypadku funkcja zdefiniowana przez użytkownika) podczas generowania sql, ale można zastąpić nazwę i schemat podczas rejestracji metody.
- Obecnie obsługiwane są tylko funkcje skalarne.
- Należy utworzyć zamapowane funkcji w bazie danych. Migracje EF Core nie zajmie się jego tworzeniem.

### <a name="self-contained-type-configuration-for-code-first"></a>Samodzielna konfiguracja typu dla kodu po raz pierwszy

W EF6 można było hermetyzować kod pierwszej konfiguracji określonego typu jednostki, wywodząc się z *EntityTypeConfiguration*. W EF Core 2.0 przywracamy ten wzorzec:

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

### <a name="dbcontext-pooling"></a>Buforowanie DbContext

Podstawowy wzorzec przy użyciu EF Core w aplikacji ASP.NET Core zwykle polega na zarejestrowaniu niestandardowego typu DbContext w systemie iniekcji zależności, a później uzyskaniu wystąpień tego typu za pomocą parametrów konstruktora w kontrolerach. Oznacza to, że dla każdego żądania tworzone jest nowe wystąpienie DbContext.

W wersji 2.0 wprowadzamy nowy sposób rejestrowania niestandardowych typów DbContext w iniekcji zależności, który w sposób przejrzysty wprowadza pulę instancji DbContext wielokrotnego pożycie. Aby użyć buforowania DbContext, `AddDbContextPool` użyj `AddDbContext` zamiast podczas rejestracji usługi:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Jeśli ta metoda jest używana, w czasie DbContext wystąpienie jest wymagane przez kontrolera najpierw sprawdzimy, czy istnieje wystąpienie dostępne w puli. Po zakończeniu przetwarzania żądania, każdy stan w wystąpieniu jest resetowany, a wystąpienie jest zwracane do puli.

Jest to koncepcyjnie podobne do sposobu, w jaki buforowanie połączeń działa w ADO.NET dostawców i ma tę zaletę, że oszczędzasz część kosztów inicjowania wystąpienia DbContext.

### <a name="limitations"></a>Ograniczenia

Nowa metoda wprowadza kilka ograniczeń co można zrobić `OnConfiguring()` w metodzie DbContext.

> [!WARNING]  
> Należy unikać korzystania z DbContext pooling, jeśli zachowasz swój własny stan (na przykład pola prywatne) w pochodnej DbContext klasy, które nie powinny być współużytkowane przez żądania. EF Core tylko zresetować stan, który jest świadomy przed dodaniem DbContext wystąpienia do puli.

### <a name="explicitly-compiled-queries"></a>Jawnie skompilowane zapytania

Jest to druga funkcja opt-in performance zaprojektowana w celu zaoferowania korzyści w scenariuszach na dużą skalę.

Ręczne lub jawnie skompilowane interfejsy API kwerend były dostępne w poprzednich wersjach EF, a także w LINQ do SQL, aby umożliwić aplikacjom buforowanie tłumaczenia zapytań, dzięki czemu mogą być obliczane tylko raz i wykonywane wiele razy.

Chociaż w ogóle EF Core można automatycznie kompilować i buforować kwerendy na podstawie mieszanej reprezentacji wyrażeń kwerendy, mechanizm ten może służyć do uzyskania niewielkiego przyrostu wydajności, pomijając obliczenia skrótu i wyszukiwania pamięci podręcznej, dzięki czemu aplikacja może używać już skompilowanej kwerendy za pomocą wywołania delegata.

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

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Dołącz może śledzić wykres nowych i istniejących elementów

EF Core obsługuje automatyczne generowanie kluczowych wartości za pomocą różnych mechanizmów. Podczas korzystania z tej funkcji, wartość jest generowany, jeśli właściwość klucza jest clr domyślne - zwykle zero lub null. Oznacza to, że wykres jednostek mogą `DbContext.Attach` `DbSet.Attach` być przekazywane do lub EF Core oznaczy `Unchanged` te jednostki, które mają klucz już ustawiony, podczas gdy te jednostki, które nie mają zestawu kluczy zostaną oznaczone jako `Added`. Ułatwia to dołączanie wykresu mieszanych nowych i istniejących jednostek podczas korzystania z wygenerowanych kluczy. `DbContext.Update`i `DbSet.Update` działają w ten sam sposób, z tą różnicą, `Unchanged`że jednostki z zestawem kluczy są oznaczone jako `Modified` zamiast .

## <a name="query"></a>Zapytanie

### <a name="improved-linq-translation"></a>Ulepszone tłumaczenie LINQ

Umożliwia pomyślne wykonanie większej liczby zapytań, przy czym więcej logiki jest obliczana w bazie danych (a nie w pamięci) i niepotrzebnie pobiera mniej danych z bazy danych.

### <a name="groupjoin-improvements"></a>Ulepszenia GroupJoin

Ta praca poprawia SQL, który jest generowany dla sprzężeń grupowych. Sprzężenia grupowe są najczęściej wynikiem zapytań podrzędnych dotyczących opcjonalnych właściwości nawigacji.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolacja ciągów w fromsql i executesqlcommand

C# 6 wprowadzono Interpolacji ciągów, funkcja, która umożliwia wyrażenia C# być bezpośrednio osadzone w literałach ciągów, zapewniając dobry sposób tworzenia ciągów w czasie wykonywania. W EF Core 2.0 dodaliśmy specjalną obsługę interpolowanych ciągów do naszych dwóch `FromSql` podstawowych interfejsów API, które akceptują surowe ciągi SQL: i `ExecuteSqlCommand`. Ta nowa obsługa umożliwia interpolacji ciągów języka C# do użycia w sposób "bezpieczny". Oznacza to, że w sposób, który chroni przed typowymi błędami iniekcji SQL, które mogą wystąpić podczas dynamicznego konstruowania sql w czasie wykonywania.

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

W tym przykładzie istnieją dwie zmienne osadzone w ciągu formatu SQL. EF Core będzie produkować następujące SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>Ef. Functions.Like()

Dodaliśmy EF. Functions właściwość, która może służyć przez EF Core lub dostawców do definiowania metod, które mapują do funkcji bazy danych lub operatorów, tak aby te mogą być wywoływane w zapytaniach LINQ. Pierwszym przykładem takiej metody jest Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Należy zauważyć, że Like() jest wyposażony w implementacji w pamięci, które mogą być przydatne podczas pracy z bazy danych w pamięci lub gdy ocena predykatu musi wystąpić po stronie klienta.

## <a name="database-management"></a>Zarządzanie bazami danych

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Hak pluralizacji do rusztowań DbContext

EF Core 2.0 wprowadza nową usługę *IPluralizer,* która jest używana do singularize nazwy typów jednostek i pluralizacji nazw DbSet. Domyślna implementacja jest nie-op, więc jest to tylko hak, gdzie ludzie mogą łatwo podłączyć własne pluralizatora.

Oto, jak to wygląda dla dewelopera, aby podłączyć we własnym pluralizatora:

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

## <a name="others"></a>Inne

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Przenieś ADO.NET dostawcę SQLite do SQLitePCL.raw

Daje nam to bardziej niezawodne rozwiązanie w witrynie Microsoft.Data.Sqlite do dystrybucji natywnych plików binarnych SQLite na różnych platformach.

### <a name="only-one-provider-per-model"></a>Tylko jeden dostawca na model

Znacznie zwiększa sposób, w jaki dostawcy mogą wchodzić w interakcje z modelem i upraszcza sposób, w jaki konwencje, adnotacje i płynne interfejsy API współpracują z różnymi dostawcami.

EF Core 2.0 będzie teraz budować inny [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) dla każdego innego dostawcy używane. Jest to zwykle przezroczyste dla aplikacji. Ułatwiło to uproszczenie interfejsów API metadanych niższego poziomu w taki sposób, że każdy dostęp `.Relational` do `.SqlServer`wspólnych `.Sqlite` *pojęć metadanych relacyjnych* jest zawsze dokonywany za pośrednictwem połączenia, zamiast , itp.

### <a name="consolidated-logging-and-diagnostics"></a>Skonsolidowane rejestrowanie i diagnostyka

Rejestrowanie (na podstawie iLogger) i diagnostyki (na podstawie DiagnosticSource) mechanizmy teraz udostępnić więcej kodu.

Identyfikatory zdarzeń dla wiadomości wysyłanych do ILogger uległy zmianie w 2.0. Identyfikatory zdarzeń są teraz unikatowe w kodzie EF Core. Te komunikaty są teraz również zgodne ze standardowym wzorcem rejestrowania strukturalnego używanym na przykład przez MVC.

Kategorie rejestratora również uległy zmianie. Istnieje teraz dobrze znany zestaw kategorii dostępnych za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).

DiagnosticSource zdarzenia teraz używać tych samych `ILogger` nazw identyfikatorów zdarzeń jako odpowiednie komunikaty.
