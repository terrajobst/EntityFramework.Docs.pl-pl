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
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="09971-102">Nowe funkcje w EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="09971-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="09971-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="09971-103">.NET Standard 2.0</span></span>

<span data-ttu-id="09971-104">EF Core jest teraz przeznaczony dla platformy .NET Standard 2.0, co oznacza, że może pracować z programem .NET Core 2.0, .NET Framework 4.6.1 i innymi bibliotekami implementuuuuujymis.</span><span class="sxs-lookup"><span data-stu-id="09971-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="09971-105">Zobacz [obsługiwane implementacje platformy .NET,](../platforms/index.md) aby uzyskać więcej informacji na temat obsługiwanych.</span><span class="sxs-lookup"><span data-stu-id="09971-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="09971-106">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="09971-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="09971-107">Dzielenie tabeli</span><span class="sxs-lookup"><span data-stu-id="09971-107">Table splitting</span></span>

<span data-ttu-id="09971-108">Teraz możliwe jest mapowanie dwóch lub więcej typów jednostek do tej samej tabeli, w której będą współużytkowane kolumny klucza podstawowego, a każdy wiersz będzie odpowiadał dwóm lub więcej jednostkom.</span><span class="sxs-lookup"><span data-stu-id="09971-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="09971-109">Aby użyć podziału tabeli, relacja identyfikująca (w której właściwości klucza obcego tworzą klucz podstawowy) musi być skonfigurowana między wszystkimi typami jednostek współużytkujących tabelę:</span><span class="sxs-lookup"><span data-stu-id="09971-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

<span data-ttu-id="09971-110">Przeczytaj [sekcję podziału tabeli, aby](xref:core/modeling/table-splitting) uzyskać więcej informacji na temat tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="09971-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="09971-111">Typy własnością</span><span class="sxs-lookup"><span data-stu-id="09971-111">Owned types</span></span>

<span data-ttu-id="09971-112">Typ jednostki należącej do jednostki może współużytkować ten sam typ platformy .NET z innym typem jednostki należącej do niej, ale ponieważ nie można go zidentyfikować tylko przez typ .NET, musi istnieć nawigacja do niego z innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="09971-112">An owned entity type can share the same .NET type with another owned entity type, but since it cannot be identified just by the .NET type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="09971-113">Jednostki zawierające nawigacji definiującej jest właścicielem.</span><span class="sxs-lookup"><span data-stu-id="09971-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="09971-114">Podczas wykonywania zapytań właściciela typy należące do właścicieli zostaną domyślnie uwzględnione.</span><span class="sxs-lookup"><span data-stu-id="09971-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="09971-115">Zgodnie z konwencją klucz podstawowy w tle zostanie utworzony dla typu należącego do właściciela i zostanie odwzorowany na tej samej tabeli co właściciel przy użyciu podziału tabeli.</span><span class="sxs-lookup"><span data-stu-id="09971-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="09971-116">Dzięki temu można używać typów posiadanych podobnie jak typy złożone są używane w EF6:</span><span class="sxs-lookup"><span data-stu-id="09971-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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

<span data-ttu-id="09971-117">Przeczytaj [sekcję dotyczącą typów jednostek należących](xref:core/modeling/owned-entities) do firmy, aby uzyskać więcej informacji na temat tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="09971-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="09971-118">Filtry zapytań na poziomie modelu</span><span class="sxs-lookup"><span data-stu-id="09971-118">Model-level query filters</span></span>

<span data-ttu-id="09971-119">EF Core 2.0 zawiera nową funkcję, którą nazywamy filtrami zapytań na poziomie modelu.</span><span class="sxs-lookup"><span data-stu-id="09971-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="09971-120">Ta funkcja umożliwia predykaty zapytania LINQ (wyrażenie logiczne zazwyczaj przekazywane do linq, gdzie operator kwerendy) mają być zdefiniowane bezpośrednio na typy jednostek w modelu metadanych (zwykle w OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="09971-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="09971-121">Takie filtry są automatycznie stosowane do wszystkich zapytań LINQ dotyczących tych typów jednostek, w tym typy jednostek, do których odwołuje się pośrednio, na przykład za pomocą funkcji Uwzględnij lub bezpośrednie odwołania do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="09971-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="09971-122">Niektóre typowe zastosowania tej funkcji to:</span><span class="sxs-lookup"><span data-stu-id="09971-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="09971-123">Usuwanie nietrwałe — typy jednostek definiuje właściwość IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="09971-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="09971-124">Multi-dzierżawy — typ jednostki definiuje TenantId właściwości.</span><span class="sxs-lookup"><span data-stu-id="09971-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="09971-125">Oto prosty przykład demonstrujący funkcję dla dwóch scenariuszy wymienionych powyżej:</span><span class="sxs-lookup"><span data-stu-id="09971-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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

<span data-ttu-id="09971-126">Definiujemy filtr na poziomie modelu, który implementuje multi-dzierżawy i `Post` soft-delete dla wystąpień typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="09971-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="09971-127">Zwróć uwagę na `DbContext` użycie właściwości `TenantId`na poziomie wystąpienia: .</span><span class="sxs-lookup"><span data-stu-id="09971-127">Note the use of a `DbContext` instance-level property: `TenantId`.</span></span> <span data-ttu-id="09971-128">Filtry na poziomie modelu użyją wartości z poprawnego wystąpienia kontekstu (czyli wystąpienia kontekstu, które wykonuje kwerendę).</span><span class="sxs-lookup"><span data-stu-id="09971-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="09971-129">Filtry mogą być wyłączone dla poszczególnych zapytań LINQ przy użyciu ignoreQueryFilters() operatora.</span><span class="sxs-lookup"><span data-stu-id="09971-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="09971-130">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="09971-130">Limitations</span></span>

- <span data-ttu-id="09971-131">Odwołania do nawigacji nie są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="09971-131">Navigation references are not allowed.</span></span> <span data-ttu-id="09971-132">Ta funkcja może zostać dodana na podstawie opinii.</span><span class="sxs-lookup"><span data-stu-id="09971-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="09971-133">Filtry można zdefiniować tylko w głównym typie jednostki hierarchii.</span><span class="sxs-lookup"><span data-stu-id="09971-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="09971-134">Mapowanie funkcji skalarnych bazy danych</span><span class="sxs-lookup"><span data-stu-id="09971-134">Database scalar function mapping</span></span>

<span data-ttu-id="09971-135">EF Core 2.0 zawiera ważny wkład [Paul Middleton,](https://github.com/pmiddleton) który umożliwia mapowanie funkcji skalarnych bazy danych do wycinków metody, dzięki czemu mogą być używane w zapytaniach LINQ i przetłumaczone na język SQL.</span><span class="sxs-lookup"><span data-stu-id="09971-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="09971-136">Oto krótki opis sposobu użycia tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="09971-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="09971-137">Zadeklarować metodę statyczną `DbContext` na swoim i `DbFunctionAttribute`opisać ją za pomocą:</span><span class="sxs-lookup"><span data-stu-id="09971-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="09971-138">Metody takie jak ten są automatycznie rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="09971-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="09971-139">Po zarejestrowaniu wywołania metody w kwerendzie LINQ można przetłumaczyć na wywołania funkcji w języku SQL:</span><span class="sxs-lookup"><span data-stu-id="09971-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="09971-140">Kilka kwestii, na które warto zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="09971-140">A few things to note:</span></span>

- <span data-ttu-id="09971-141">Zgodnie z konwencją nazwa metody jest używana jako nazwa funkcji (w tym przypadku funkcja zdefiniowana przez użytkownika) podczas generowania sql, ale można zastąpić nazwę i schemat podczas rejestracji metody.</span><span class="sxs-lookup"><span data-stu-id="09971-141">By convention the name of the method is used as the name of a function (in this case a user-defined function) when generating the SQL, but you can override the name and schema during method registration.</span></span>
- <span data-ttu-id="09971-142">Obecnie obsługiwane są tylko funkcje skalarne.</span><span class="sxs-lookup"><span data-stu-id="09971-142">Currently only scalar functions are supported.</span></span>
- <span data-ttu-id="09971-143">Należy utworzyć zamapowane funkcji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="09971-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="09971-144">Migracje EF Core nie zajmie się jego tworzeniem.</span><span class="sxs-lookup"><span data-stu-id="09971-144">EF Core migrations will not take care of creating it.</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="09971-145">Samodzielna konfiguracja typu dla kodu po raz pierwszy</span><span class="sxs-lookup"><span data-stu-id="09971-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="09971-146">W EF6 można było hermetyzować kod pierwszej konfiguracji określonego typu jednostki, wywodząc się z *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="09971-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="09971-147">W EF Core 2.0 przywracamy ten wzorzec:</span><span class="sxs-lookup"><span data-stu-id="09971-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="09971-148">Wysoka wydajność</span><span class="sxs-lookup"><span data-stu-id="09971-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="09971-149">Buforowanie DbContext</span><span class="sxs-lookup"><span data-stu-id="09971-149">DbContext pooling</span></span>

<span data-ttu-id="09971-150">Podstawowy wzorzec przy użyciu EF Core w aplikacji ASP.NET Core zwykle polega na zarejestrowaniu niestandardowego typu DbContext w systemie iniekcji zależności, a później uzyskaniu wystąpień tego typu za pomocą parametrów konstruktora w kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="09971-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="09971-151">Oznacza to, że dla każdego żądania tworzone jest nowe wystąpienie DbContext.</span><span class="sxs-lookup"><span data-stu-id="09971-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="09971-152">W wersji 2.0 wprowadzamy nowy sposób rejestrowania niestandardowych typów DbContext w iniekcji zależności, który w sposób przejrzysty wprowadza pulę instancji DbContext wielokrotnego pożycie.</span><span class="sxs-lookup"><span data-stu-id="09971-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="09971-153">Aby użyć buforowania DbContext, `AddDbContextPool` użyj `AddDbContext` zamiast podczas rejestracji usługi:</span><span class="sxs-lookup"><span data-stu-id="09971-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="09971-154">Jeśli ta metoda jest używana, w czasie DbContext wystąpienie jest wymagane przez kontrolera najpierw sprawdzimy, czy istnieje wystąpienie dostępne w puli.</span><span class="sxs-lookup"><span data-stu-id="09971-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="09971-155">Po zakończeniu przetwarzania żądania, każdy stan w wystąpieniu jest resetowany, a wystąpienie jest zwracane do puli.</span><span class="sxs-lookup"><span data-stu-id="09971-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="09971-156">Jest to koncepcyjnie podobne do sposobu, w jaki buforowanie połączeń działa w ADO.NET dostawców i ma tę zaletę, że oszczędzasz część kosztów inicjowania wystąpienia DbContext.</span><span class="sxs-lookup"><span data-stu-id="09971-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="09971-157">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="09971-157">Limitations</span></span>

<span data-ttu-id="09971-158">Nowa metoda wprowadza kilka ograniczeń co można zrobić `OnConfiguring()` w metodzie DbContext.</span><span class="sxs-lookup"><span data-stu-id="09971-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="09971-159">Należy unikać korzystania z DbContext pooling, jeśli zachowasz swój własny stan (na przykład pola prywatne) w pochodnej DbContext klasy, które nie powinny być współużytkowane przez żądania.</span><span class="sxs-lookup"><span data-stu-id="09971-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="09971-160">EF Core tylko zresetować stan, który jest świadomy przed dodaniem DbContext wystąpienia do puli.</span><span class="sxs-lookup"><span data-stu-id="09971-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="09971-161">Jawnie skompilowane zapytania</span><span class="sxs-lookup"><span data-stu-id="09971-161">Explicitly compiled queries</span></span>

<span data-ttu-id="09971-162">Jest to druga funkcja opt-in performance zaprojektowana w celu zaoferowania korzyści w scenariuszach na dużą skalę.</span><span class="sxs-lookup"><span data-stu-id="09971-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="09971-163">Ręczne lub jawnie skompilowane interfejsy API kwerend były dostępne w poprzednich wersjach EF, a także w LINQ do SQL, aby umożliwić aplikacjom buforowanie tłumaczenia zapytań, dzięki czemu mogą być obliczane tylko raz i wykonywane wiele razy.</span><span class="sxs-lookup"><span data-stu-id="09971-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="09971-164">Chociaż w ogóle EF Core można automatycznie kompilować i buforować kwerendy na podstawie mieszanej reprezentacji wyrażeń kwerendy, mechanizm ten może służyć do uzyskania niewielkiego przyrostu wydajności, pomijając obliczenia skrótu i wyszukiwania pamięci podręcznej, dzięki czemu aplikacja może używać już skompilowanej kwerendy za pomocą wywołania delegata.</span><span class="sxs-lookup"><span data-stu-id="09971-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="09971-165">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="09971-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="09971-166">Dołącz może śledzić wykres nowych i istniejących elementów</span><span class="sxs-lookup"><span data-stu-id="09971-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="09971-167">EF Core obsługuje automatyczne generowanie kluczowych wartości za pomocą różnych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="09971-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="09971-168">Podczas korzystania z tej funkcji, wartość jest generowany, jeśli właściwość klucza jest clr domyślne - zwykle zero lub null.</span><span class="sxs-lookup"><span data-stu-id="09971-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="09971-169">Oznacza to, że wykres jednostek mogą `DbContext.Attach` `DbSet.Attach` być przekazywane do lub EF Core oznaczy `Unchanged` te jednostki, które mają klucz już ustawiony, podczas gdy te jednostki, które nie mają zestawu kluczy zostaną oznaczone jako `Added`.</span><span class="sxs-lookup"><span data-stu-id="09971-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="09971-170">Ułatwia to dołączanie wykresu mieszanych nowych i istniejących jednostek podczas korzystania z wygenerowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="09971-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="09971-171">`DbContext.Update`i `DbSet.Update` działają w ten sam sposób, z tą różnicą, `Unchanged`że jednostki z zestawem kluczy są oznaczone jako `Modified` zamiast .</span><span class="sxs-lookup"><span data-stu-id="09971-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="09971-172">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="09971-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="09971-173">Ulepszone tłumaczenie LINQ</span><span class="sxs-lookup"><span data-stu-id="09971-173">Improved LINQ translation</span></span>

<span data-ttu-id="09971-174">Umożliwia pomyślne wykonanie większej liczby zapytań, przy czym więcej logiki jest obliczana w bazie danych (a nie w pamięci) i niepotrzebnie pobiera mniej danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="09971-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="09971-175">Ulepszenia GroupJoin</span><span class="sxs-lookup"><span data-stu-id="09971-175">GroupJoin improvements</span></span>

<span data-ttu-id="09971-176">Ta praca poprawia SQL, który jest generowany dla sprzężeń grupowych.</span><span class="sxs-lookup"><span data-stu-id="09971-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="09971-177">Sprzężenia grupowe są najczęściej wynikiem zapytań podrzędnych dotyczących opcjonalnych właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="09971-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="09971-178">Interpolacja ciągów w fromsql i executesqlcommand</span><span class="sxs-lookup"><span data-stu-id="09971-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="09971-179">C# 6 wprowadzono Interpolacji ciągów, funkcja, która umożliwia wyrażenia C# być bezpośrednio osadzone w literałach ciągów, zapewniając dobry sposób tworzenia ciągów w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="09971-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="09971-180">W EF Core 2.0 dodaliśmy specjalną obsługę interpolowanych ciągów do naszych dwóch `FromSql` podstawowych interfejsów API, które akceptują surowe ciągi SQL: i `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="09971-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="09971-181">Ta nowa obsługa umożliwia interpolacji ciągów języka C# do użycia w sposób "bezpieczny".</span><span class="sxs-lookup"><span data-stu-id="09971-181">This new support allows C# string interpolation to be used in a "safe" manner.</span></span> <span data-ttu-id="09971-182">Oznacza to, że w sposób, który chroni przed typowymi błędami iniekcji SQL, które mogą wystąpić podczas dynamicznego konstruowania sql w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="09971-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="09971-183">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="09971-183">Here is an example:</span></span>

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

<span data-ttu-id="09971-184">W tym przykładzie istnieją dwie zmienne osadzone w ciągu formatu SQL.</span><span class="sxs-lookup"><span data-stu-id="09971-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="09971-185">EF Core będzie produkować następujące SQL:</span><span class="sxs-lookup"><span data-stu-id="09971-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="09971-186">Ef. Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="09971-186">EF.Functions.Like()</span></span>

<span data-ttu-id="09971-187">Dodaliśmy EF. Functions właściwość, która może służyć przez EF Core lub dostawców do definiowania metod, które mapują do funkcji bazy danych lub operatorów, tak aby te mogą być wywoływane w zapytaniach LINQ.</span><span class="sxs-lookup"><span data-stu-id="09971-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="09971-188">Pierwszym przykładem takiej metody jest Like():</span><span class="sxs-lookup"><span data-stu-id="09971-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="09971-189">Należy zauważyć, że Like() jest wyposażony w implementacji w pamięci, które mogą być przydatne podczas pracy z bazy danych w pamięci lub gdy ocena predykatu musi wystąpić po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="09971-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="09971-190">Zarządzanie bazami danych</span><span class="sxs-lookup"><span data-stu-id="09971-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="09971-191">Hak pluralizacji do rusztowań DbContext</span><span class="sxs-lookup"><span data-stu-id="09971-191">Pluralization hook for DbContext scaffolding</span></span>

<span data-ttu-id="09971-192">EF Core 2.0 wprowadza nową usługę *IPluralizer,* która jest używana do singularize nazwy typów jednostek i pluralizacji nazw DbSet.</span><span class="sxs-lookup"><span data-stu-id="09971-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="09971-193">Domyślna implementacja jest nie-op, więc jest to tylko hak, gdzie ludzie mogą łatwo podłączyć własne pluralizatora.</span><span class="sxs-lookup"><span data-stu-id="09971-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="09971-194">Oto, jak to wygląda dla dewelopera, aby podłączyć we własnym pluralizatora:</span><span class="sxs-lookup"><span data-stu-id="09971-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="09971-195">Inne</span><span class="sxs-lookup"><span data-stu-id="09971-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="09971-196">Przenieś ADO.NET dostawcę SQLite do SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="09971-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>

<span data-ttu-id="09971-197">Daje nam to bardziej niezawodne rozwiązanie w witrynie Microsoft.Data.Sqlite do dystrybucji natywnych plików binarnych SQLite na różnych platformach.</span><span class="sxs-lookup"><span data-stu-id="09971-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="09971-198">Tylko jeden dostawca na model</span><span class="sxs-lookup"><span data-stu-id="09971-198">Only one provider per model</span></span>

<span data-ttu-id="09971-199">Znacznie zwiększa sposób, w jaki dostawcy mogą wchodzić w interakcje z modelem i upraszcza sposób, w jaki konwencje, adnotacje i płynne interfejsy API współpracują z różnymi dostawcami.</span><span class="sxs-lookup"><span data-stu-id="09971-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="09971-200">EF Core 2.0 będzie teraz budować inny [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) dla każdego innego dostawcy używane.</span><span class="sxs-lookup"><span data-stu-id="09971-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="09971-201">Jest to zwykle przezroczyste dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09971-201">This is usually transparent to the application.</span></span> <span data-ttu-id="09971-202">Ułatwiło to uproszczenie interfejsów API metadanych niższego poziomu w taki sposób, że każdy dostęp `.Relational` do `.SqlServer`wspólnych `.Sqlite` *pojęć metadanych relacyjnych* jest zawsze dokonywany za pośrednictwem połączenia, zamiast , itp.</span><span class="sxs-lookup"><span data-stu-id="09971-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="09971-203">Skonsolidowane rejestrowanie i diagnostyka</span><span class="sxs-lookup"><span data-stu-id="09971-203">Consolidated logging and diagnostics</span></span>

<span data-ttu-id="09971-204">Rejestrowanie (na podstawie iLogger) i diagnostyki (na podstawie DiagnosticSource) mechanizmy teraz udostępnić więcej kodu.</span><span class="sxs-lookup"><span data-stu-id="09971-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="09971-205">Identyfikatory zdarzeń dla wiadomości wysyłanych do ILogger uległy zmianie w 2.0.</span><span class="sxs-lookup"><span data-stu-id="09971-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="09971-206">Identyfikatory zdarzeń są teraz unikatowe w kodzie EF Core.</span><span class="sxs-lookup"><span data-stu-id="09971-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="09971-207">Te komunikaty są teraz również zgodne ze standardowym wzorcem rejestrowania strukturalnego używanym na przykład przez MVC.</span><span class="sxs-lookup"><span data-stu-id="09971-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="09971-208">Kategorie rejestratora również uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="09971-208">Logger categories have also changed.</span></span> <span data-ttu-id="09971-209">Istnieje teraz dobrze znany zestaw kategorii dostępnych za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="09971-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="09971-210">DiagnosticSource zdarzenia teraz używać tych samych `ILogger` nazw identyfikatorów zdarzeń jako odpowiednie komunikaty.</span><span class="sxs-lookup"><span data-stu-id="09971-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
