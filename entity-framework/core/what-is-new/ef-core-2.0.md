---
title: What's new in EF Core 2.0 — EF Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 4b319e7d4571e5e32ae7470601345e6f98807551
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911544"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="a4b7f-102">Nowe funkcje programu EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="a4b7f-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="a4b7f-103">.NET standard 2.0</span><span class="sxs-lookup"><span data-stu-id="a4b7f-103">.NET Standard 2.0</span></span>
<span data-ttu-id="a4b7f-104">EF Core teraz jest przeznaczony dla .NET Standard 2.0, co oznacza, że można pracować przy użyciu platformy .NET Core 2.0, .NET Framework 4.6.1 i innych bibliotek, które implementują .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="a4b7f-105">Zobacz [obsługiwane implementacje platformy .NET](../platforms/index.md) więcej informacji o tym, co jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="a4b7f-106">Modelowanie</span><span class="sxs-lookup"><span data-stu-id="a4b7f-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="a4b7f-107">Dzielenie tabeli</span><span class="sxs-lookup"><span data-stu-id="a4b7f-107">Table splitting</span></span>

<span data-ttu-id="a4b7f-108">Teraz jest możliwa do mapowania dwóch lub więcej typów jednostek do tej samej tabeli, gdzie zostaną udostępnione kolumn klucza podstawowego, a każdy wiersz odpowiada dwóch lub więcej podmiotów.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="a4b7f-109">Można użyć tabeli podział identyfikującą relację (gdzie właściwości klucza obcego tworzą klucz podstawowy) musi być skonfigurowany między wszystkie typy jednostek, udostępnianie w tabeli:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="a4b7f-110">Typy należące do firmy</span><span class="sxs-lookup"><span data-stu-id="a4b7f-110">Owned types</span></span>

<span data-ttu-id="a4b7f-111">Typ jednostki należące do firmy można udostępnić tego samego typu CLR z innym typem jednostki należące do firmy, ale ponieważ nie można zidentyfikować przez typ CLR musi istnieć nawigację do niego z innego typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="a4b7f-112">Obiekt zawierający definiujące nawigacji jest właścicielem.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="a4b7f-113">Podczas wykonywania zapytań dotyczących właściciela należących do typów będą uwzględniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="a4b7f-114">Zgodnie z Konwencją klucza podstawowego w tle zostanie utworzony dla typu należące do firmy i będzie można zamapować na tej samej tabeli jako właściciel przy użyciu dzielenie tabeli.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="a4b7f-115">Dzięki temu Użyj należące do typów podobnie jak złożonych typów są używane w EF6:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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
<span data-ttu-id="a4b7f-116">Odczyt [sekcji posiadane typy jednostek](xref:core/modeling/owned-entities) Aby uzyskać więcej informacji na temat tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-116">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="a4b7f-117">Filtry na poziomie modelu kwerendy</span><span class="sxs-lookup"><span data-stu-id="a4b7f-117">Model-level query filters</span></span>

<span data-ttu-id="a4b7f-118">EF Core 2.0 zawiera nową funkcję nazywamy filtry kwerendy na poziomie modelu.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-118">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="a4b7f-119">Ta funkcja umożliwia predykatów zapytań LINQ (wyrażenia logicznego zwykle są przekazywane do operatora zapytania LINQ w przypadku gdy) należy zdefiniować bezpośrednio na typy jednostek w modelu metadanych (zwykle w OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="a4b7f-119">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="a4b7f-120">Takie filtry są automatycznie stosowane do żadnych zapytań LINQ, obejmujące tych typów jednostek, w tym odwołania do właściwości nawigacji odwołanie pośrednio, takie jak przy użyciu Include lub bezpośredniego typów jednostek.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-120">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="a4b7f-121">Niektóre typowe aplikacje tej funkcji są następujące:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-121">Some common applications of this feature are:</span></span>

- <span data-ttu-id="a4b7f-122">Usuwanie nietrwałe — typy jednostek definiuje właściwość IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-122">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="a4b7f-123">Wielodostępność — typ jednostki definiuje właściwość identyfikatora dzierżawcy.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-123">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="a4b7f-124">Poniżej przedstawiono prosty przykład Demonstrowanie funkcji w przypadku dwóch scenariuszy wymienionych powyżej:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-124">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
<span data-ttu-id="a4b7f-125">Definiujemy filtr na poziomie modelu, który implementuje wielodostępu i usuwania nietrwałego dla wystąpienia elementu ```Post``` typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-125">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="a4b7f-126">Zwróć uwagę na użycie właściwości poziomu wystąpienia typu DbContext: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-126">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="a4b7f-127">Filtry na poziomie modelu będzie używać wartości z wystąpienia poprawnego kontekstu.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-127">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="a4b7f-128">Czyli ten, który wykonuje zapytanie.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-128">I.e. the one that is executing the query.</span></span>

<span data-ttu-id="a4b7f-129">Filtry mogą być wyłączone dla poszczególnych zapytań LINQ, za pomocą operatora IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="a4b7f-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="a4b7f-130">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="a4b7f-130">Limitations</span></span>

- <span data-ttu-id="a4b7f-131">Nawigacja odwołania nie są dozwolone.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-131">Navigation references are not allowed.</span></span> <span data-ttu-id="a4b7f-132">Ta funkcja mogą być dodawane w oparciu o opinie.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="a4b7f-133">Filtry można zdefiniować tylko w katalogu głównym typ jednostki dla hierarchii.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="a4b7f-134">Mapowanie funkcji skalarnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="a4b7f-134">Database scalar function mapping</span></span>

<span data-ttu-id="a4b7f-135">EF Core 2.0 obejmuje ważnym wkładem z [Paul Middleton](https://github.com/pmiddleton) umożliwiająca mapowania funkcji skalarnych bazy danych do metody zastępczych, dzięki czemu mogą być używane w kwerendach LINQ i przetłumaczone do bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="a4b7f-136">Poniżej przedstawiono krótki opis sposobu użycia funkcji:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="a4b7f-137">Zadeklaruj metodę statyczną na Twoje `DbContext` i Adnotuj ją za pomocą `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="a4b7f-138">Metod, takich jak to są automatycznie rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="a4b7f-139">Po zarejestrowaniu wywołania metody w zapytaniu programu LINQ mogą być tłumaczone do wywołania funkcji w języku SQL:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="a4b7f-140">Kilka kwestii, które należy zwrócić uwagę:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-140">A few things to note:</span></span>

- <span data-ttu-id="a4b7f-141">Zgodnie z Konwencją Nazwa metody jest używana jako nazwa funkcji (w tym przypadku funkcję zdefiniowaną przez użytkownika) podczas generowania SQL, ale można zastąpić nazwę i schematu podczas rejestracji — metoda</span><span class="sxs-lookup"><span data-stu-id="a4b7f-141">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="a4b7f-142">Obecnie obsługiwane są tylko funkcje skalarne</span><span class="sxs-lookup"><span data-stu-id="a4b7f-142">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="a4b7f-143">W bazie danych, np. programu EF Core migracje nie zajmie się jego tworzenia należy utworzyć zamapowanego — funkcja</span><span class="sxs-lookup"><span data-stu-id="a4b7f-143">You must create the mapped function in the database, e.g. EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="a4b7f-144">Konfiguracja typu niezależna kodu pierwszy</span><span class="sxs-lookup"><span data-stu-id="a4b7f-144">Self-contained type configuration for code first</span></span>

<span data-ttu-id="a4b7f-145">W EF6 było możliwe do hermetyzacji kodu konfiguracji pierwszego określonego typu, wynikające z *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-145">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="a4b7f-146">W programie EF Core 2.0 możemy ponownie także tego wzorca:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-146">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="a4b7f-147">Wysoka wydajność</span><span class="sxs-lookup"><span data-stu-id="a4b7f-147">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="a4b7f-148">Buforowanie typu DbContext</span><span class="sxs-lookup"><span data-stu-id="a4b7f-148">DbContext pooling</span></span>

<span data-ttu-id="a4b7f-149">Podstawowy wzorzec przy użyciu programu EF Core w aplikacji ASP.NET Core zwykle obejmuje rejestrowania niestandardowego typu DbContext w systemie iniekcji zależności i później uzyskanie wystąpień tego typu za pomocą konstruktora parametrów w kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-149">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="a4b7f-150">Oznacza to, że nowe wystąpienie klasy kontekstu DbContext jest tworzony dla każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-150">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="a4b7f-151">W wersji 2.0 wprowadzamy nowy sposób rejestrowania niestandardowego typu DbContext w wstrzykiwanie zależności, które wprowadza w sposób niewidoczny dla użytkownika pulę wystąpień wielokrotnego użytku DbContext.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-151">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="a4b7f-152">Aby użyć typu DbContext buforowanie, użyj ```AddDbContextPool``` zamiast ```AddDbContext``` podczas rejestracji usługi:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-152">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="a4b7f-153">Jeśli ta metoda jest używana, w momencie wystąpienia typu DbContext jest wymagany przez kontrolera, że firma Microsoft będzie najpierw sprawdź, czy wystąpienie dostępne w puli.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-153">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="a4b7f-154">Po Kończenie znajdujących się przetwarzanie żądań, każdy stan w wystąpieniu jest resetowany, a wystąpienie jest zwracane do puli.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-154">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="a4b7f-155">Jest to zachowuje się podobnie jak jak buforowanie połączeń działa w dostawcy ADO.NET i ma tę zaletę zapisywania koszt inicjowania wystąpienia typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-155">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="a4b7f-156">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="a4b7f-156">Limitations</span></span>

<span data-ttu-id="a4b7f-157">Nowa metoda wprowadza pewne ograniczenia na co można zrobić ```OnConfiguring()``` metoda kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-157">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="a4b7f-158">Należy unikać buforowanie DbContext Jeśli to Ty masz własne stanu (np. pola prywatne) w swojej otrzymanej klasy DbContext, który nie powinien być współużytkowane przez wiele żądań.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-158">Avoid using DbContext Pooling if you maintain your own state (e.g. private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="a4b7f-159">EF Core tylko spowoduje zresetowanie stanu który zna przed dodaniem wystąpienia typu DbContext do puli.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-159">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="a4b7f-160">Zapytania skompilowane jawnie</span><span class="sxs-lookup"><span data-stu-id="a4b7f-160">Explicitly compiled queries</span></span>

<span data-ttu-id="a4b7f-161">Jest to drugi zdecydować się na wydajności funkcji oferują korzyści w scenariuszach o dużej skali.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-161">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="a4b7f-162">Ręczne lub jawnie skompilowanych zapytanie interfejsy API zostały dostępne w poprzednich wersjach programu EF, a także w składniku LINQ to SQL, aby umożliwić aplikacjom tak, aby może zostać obliczony tylko raz w pamięci podręcznej tłumaczenie zapytań i wykonywane wiele razy.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-162">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="a4b7f-163">Chociaż ogólnie rzecz biorąc programu EF Core można automatycznie kompilują i kwerendy oparte na mieszanych reprezentacja wyrażenia zapytania w pamięci podręcznej, mechanizm ten może służyć do uzyskania małych są bardziej wydajne, pomijając obliczania skrótu i przeszukiwania pamięci podręcznej, dzięki czemu aplikacji do korzystania z zapytania już skompilowane, za pośrednictwem wywołania delegata.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-163">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="a4b7f-164">Śledzenie zmian</span><span class="sxs-lookup"><span data-stu-id="a4b7f-164">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="a4b7f-165">Dołącz można śledzić wykres nowych i istniejących jednostek</span><span class="sxs-lookup"><span data-stu-id="a4b7f-165">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="a4b7f-166">EF Core obsługuje automatyczne generowanie wartości klucza przy użyciu różnych mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-166">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="a4b7f-167">Podczas używania tej funkcji, wartość jest generowany, gdy właściwość klucza jest ustawieniem domyślnym CLR — zwykle zero lub wartość null.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-167">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="a4b7f-168">Oznacza to, że wykres jednostki mogą być przekazywane do `DbContext.Attach` lub `DbSet.Attach` i programem EF Core spowoduje oznaczenie tych obiektów, które mają klucz, który został już ustawiony jako `Unchanged` podczas tych jednostek, które nie mają zestawu kluczy zostaną oznaczone jako `Added`.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-168">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="a4b7f-169">Ułatwia można dołączyć wykres mieszane nowych i istniejących jednostek, korzystając z wygenerowanych kluczy.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-169">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="a4b7f-170">`DbContext.Update` i `DbSet.Update` działają w taki sam sposób, z tą różnicą, że jednostki z zestawu kluczy, są oznaczone jako `Modified` zamiast `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-170">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="a4b7f-171">Zapytanie</span><span class="sxs-lookup"><span data-stu-id="a4b7f-171">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="a4b7f-172">Ulepszone LINQ tłumaczenia</span><span class="sxs-lookup"><span data-stu-id="a4b7f-172">Improved LINQ Translation</span></span>

<span data-ttu-id="a4b7f-173">Umożliwia więcej zapytań pomyślnie wykonać logiką więcej ocenianego w bazie danych (a nie w pamięci), a dane niepotrzebnie pobrane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-173">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="a4b7f-174">Groupjoin — ulepszenia</span><span class="sxs-lookup"><span data-stu-id="a4b7f-174">GroupJoin improvements</span></span>

<span data-ttu-id="a4b7f-175">Ta Praca zwiększa SQL Server, który jest generowany dla sprzężenia grupowane.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-175">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="a4b7f-176">Sprzężenia grupowane w większości przypadków są wynikiem podzapytaniami właściwości nawigacji opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-176">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="a4b7f-177">Interpolacja ciągów w FromSql i ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="a4b7f-177">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="a4b7f-178">C# 6 wprowadzono Interpolacja ciągów funkcja, która umożliwia wyrażeń języka C# do osadzenia bezpośrednio Literały ciągu, dzięki czemu ładny building ciągów w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-178">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="a4b7f-179">W programie EF Core 2.0 dodaliśmy specjalne obsługę ciągów interpolowanych do naszych dwa podstawowe interfejsów API, które akceptują pierwotne ciągów SQL: ```FromSql``` i ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-179">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="a4b7f-180">Ta obsługa nowych umożliwia interpolacji ciągu C# ma być używany w sposób "bezpieczne".</span><span class="sxs-lookup"><span data-stu-id="a4b7f-180">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="a4b7f-181">Np. w sposób, który chroni przed typowych pomyłek iniekcja SQL, które mogą wystąpić podczas dynamiczne budowanie SQL w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-181">I.e. in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="a4b7f-182">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-182">Here is an example:</span></span>

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

<span data-ttu-id="a4b7f-183">W tym przykładzie istnieją dwie zmienne osadzone w ciągu formatu SQL.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-183">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="a4b7f-184">EF Core generuje następujące instrukcje SQL:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-184">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="a4b7f-185">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="a4b7f-185">EF.Functions.Like()</span></span>

<span data-ttu-id="a4b7f-186">Dodaliśmy EF. Właściwości funkcji, która może być używany przez programu EF Core lub dostawców do definiowania metod mapowane na funkcje bazy danych lub operatorów, tak, aby te mogą być wywoływane w zapytaniach LINQ.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-186">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="a4b7f-187">Pierwszy przykład taka metoda jest Like():</span><span class="sxs-lookup"><span data-stu-id="a4b7f-187">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="a4b7f-188">Należy pamiętać, że Like() pochodzi z implementacją w pamięci, które mogą być przydatne podczas pracy w bazie danych w pamięci lub oceny predykatu musi nastąpić po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-188">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="a4b7f-189">Zarządzanie bazą danych</span><span class="sxs-lookup"><span data-stu-id="a4b7f-189">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="a4b7f-190">Pluralizacja podłączania do tworzenia szkieletów DbContext</span><span class="sxs-lookup"><span data-stu-id="a4b7f-190">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="a4b7f-191">EF Core 2.0 wprowadzono nowy *IPluralizer* usługa, która jest używana do końcówek jednostki wpisz nazwy użytkowników i DbSet nazwy w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-191">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="a4b7f-192">Domyślna implementacja jest pusta, więc jest to po prostu podłączania, gdzie osoby zajmujące się łatwo podłączyć w ich własnych pluralizer.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-192">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="a4b7f-193">Poniżej przedstawiono, jak to wygląda dla dewelopera, można dołączyć w ich własnych pluralizer:</span><span class="sxs-lookup"><span data-stu-id="a4b7f-193">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="a4b7f-194">Inne osoby</span><span class="sxs-lookup"><span data-stu-id="a4b7f-194">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="a4b7f-195">Przenieś dostawcy ADO.NET SQLite SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="a4b7f-195">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="a4b7f-196">To daje nam bardziej niezawodne rozwiązanie w Microsoft.Data.Sqlite dystrybucji natywnych SQLite plików binarnych na różnych platformach.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-196">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="a4b7f-197">Tylko jeden dostawca na modelu</span><span class="sxs-lookup"><span data-stu-id="a4b7f-197">Only one provider per model</span></span>
<span data-ttu-id="a4b7f-198">Znacznie rozszerzają sposobu interakcji z modelem dostawców i upraszcza sposób działania Konwencji, adnotacji i płynnych interfejsów API za pomocą różnych dostawców.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-198">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="a4b7f-199">EF Core 2.0 będą teraz tworzyć inną [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) dla każdego innego dostawcy używane.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-199">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="a4b7f-200">Jest to zazwyczaj niewidoczna dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-200">This is usually transparent to the application.</span></span> <span data-ttu-id="a4b7f-201">Ma to ułatwione uproszczenia metadanych niskiego poziomu interfejsy API, taki sposób, że dowolny dostęp do *typowe pojęcia relacyjnych metadanych* zawsze jest wykonywane za pomocą wywołania `.Relational` zamiast `.SqlServer`, `.Sqlite`itp.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-201">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="a4b7f-202">Skonsolidowane rejestrowania i diagnostyki</span><span class="sxs-lookup"><span data-stu-id="a4b7f-202">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="a4b7f-203">Rejestrowanie (oparte na ILogger) i mechanizmy diagnostyki (oparte na DiagnosticSource) teraz udostępnić więcej kodu.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-203">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="a4b7f-204">Identyfikatory zdarzeń dla wiadomości wysyłanych do ILogger zostały zmienione w wersji 2.0.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-204">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="a4b7f-205">Identyfikatory zdarzeń obecnie są unikatowe w obrębie kod programem EF Core.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-205">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="a4b7f-206">Te komunikaty teraz również wykonać standardowego wzorca dla rejestrowaniem strukturalnym używane przez, na przykład MVC.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-206">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="a4b7f-207">Kategorie rejestratora również zostały zmienione.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-207">Logger categories have also changed.</span></span> <span data-ttu-id="a4b7f-208">Ma teraz dobrze znanego zestawu kategorii dostępne za pośrednictwem [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="a4b7f-208">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="a4b7f-209">Zdarzenia DiagnosticSource teraz użyć takich samych nazwach identyfikator zdarzenia odpowiadającego `ILogger` wiadomości.</span><span class="sxs-lookup"><span data-stu-id="a4b7f-209">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
