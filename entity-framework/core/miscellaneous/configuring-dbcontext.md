---
title: Konfigurowanie DbContext - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152393"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="1b550-102">Konfigurowanie obiektu DbContext</span><span class="sxs-lookup"><span data-stu-id="1b550-102">Configuring a DbContext</span></span>

<span data-ttu-id="1b550-103">W tym artykule przedstawiono wzorców podstawowych konfigurowania `DbContext` za pośrednictwem `DbContextOptions` nawiązać połączenia z bazą danych przy użyciu określonego dostawcy EF Core i opcjonalnie zachowania.</span><span class="sxs-lookup"><span data-stu-id="1b550-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="1b550-104">Konfiguracja obiektu DbContext czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="1b550-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="1b550-105">Czasu projektowania EF podstawowych narzędzi, takich jak [migracje](xref:core/managing-schemas/migrations/index) muszą mieć możliwość odnajdywania i Utwórz wystąpienie pracy `DbContext` typu w celu zbierania szczegóły dotyczące typów jednostek aplikacji oraz sposobu mapowania ich na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1b550-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="1b550-106">Ten proces może być automatyczna, tak długo, jak łatwo utworzyć narzędzie `DbContext` w taki sposób, że będzie on podobnie skonfigurowany sposób może zostać skonfigurowana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="1b550-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="1b550-107">Podczas żadnych wzorzec, który zawiera informacje o konfiguracji niezbędne do `DbContext` może działać w czasie wykonywania, narzędzi, które wymagają przy użyciu `DbContext` w czasie projektowania może pracować tylko z ograniczoną liczbą wzorców.</span><span class="sxs-lookup"><span data-stu-id="1b550-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="1b550-108">Te są opisane bardziej szczegółowo w [tworzenie kontekstu w czasie projektowania](xref:core/miscellaneous/cli/dbcontext-creation) sekcji.</span><span class="sxs-lookup"><span data-stu-id="1b550-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="1b550-109">Konfigurowanie DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="1b550-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="1b550-110">`DbContext` musi być wystąpieniem `DbContextOptions` w celu wykonywania pracy.</span><span class="sxs-lookup"><span data-stu-id="1b550-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="1b550-111">`DbContextOptions` Wystąpienia niesie informacje o konfiguracji, takich jak:</span><span class="sxs-lookup"><span data-stu-id="1b550-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="1b550-112">Dostawca bazy danych do użycia, zazwyczaj wybierana przez wywołanie metody, takie jak `UseSqlServer` lub `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="1b550-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="1b550-113">Wszystkie niezbędne parametry lub identyfikator wystąpienia bazy danych zazwyczaj przekazanego jako argument do metody wyboru dostawcy wymienione powyżej</span><span class="sxs-lookup"><span data-stu-id="1b550-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="1b550-114">Wszystkie selektory zachowanie opcjonalne poziomie dostawcy, zwykle także powiązane wewnątrz wywołania metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="1b550-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="1b550-115">Wszelkie ogólne selektorów zachowanie EF Core, zwykle łańcuchowej po lub przed wywołaniem metody selektora dostawcy</span><span class="sxs-lookup"><span data-stu-id="1b550-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="1b550-116">Poniższy przykład konfiguruje `DbContextOptions` korzysta z dostawcy programu SQL Server, połączenie zawarte w `connectionString` zmiennej, limit czasu polecenia poziomie dostawcy i selektor zachowanie EF Core sprawia, że wszystkie zapytania wykonywane w `DbContext` [śledzenia nie](xref:core/querying/tracking#no-tracking-queries) domyślnie:</span><span class="sxs-lookup"><span data-stu-id="1b550-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="1b550-117">Metody selektora dostawcy i innych metod selektora zachowanie wymienione powyżej są metody rozszerzenia na `DbContextOptions` lub klasy opcji specyficznych dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="1b550-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="1b550-118">Aby uzyskać dostęp do tych metod rozszerzenia może być konieczne przestrzeń nazw (zazwyczaj `Microsoft.EntityFrameworkCore`) w zakres i zawierać pakiet dodatkowe zależności w projekcie.</span><span class="sxs-lookup"><span data-stu-id="1b550-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="1b550-119">`DbContextOptions` Mogą być dostarczane do `DbContext` przez zastąpienie `OnConfiguring` metody lub zewnętrznie za pośrednictwem argumentów konstruktora.</span><span class="sxs-lookup"><span data-stu-id="1b550-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="1b550-120">Jeśli używana zarówno `OnConfiguring` ostatnio zastosowane i mogą zastąpić opcje przekazany do argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="1b550-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="1b550-121">Argument konstruktora</span><span class="sxs-lookup"><span data-stu-id="1b550-121">Constructor argument</span></span>

<span data-ttu-id="1b550-122">Kontekst kodu za pomocą konstruktora:</span><span class="sxs-lookup"><span data-stu-id="1b550-122">Context code with constructor:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="1b550-123">Podstawowy Konstruktor DbContext również zaakceptowanie wersji nieogólnego `DbContextOptions`, ale przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji o wiele typów kontekstu.</span><span class="sxs-lookup"><span data-stu-id="1b550-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="1b550-124">Kod aplikacji, można zainicjować na podstawie argumentów konstruktora:</span><span class="sxs-lookup"><span data-stu-id="1b550-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="1b550-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="1b550-125">OnConfiguring</span></span>

<span data-ttu-id="1b550-126">Kontekst kodu za pomocą `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="1b550-126">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="1b550-127">Kod aplikacji w celu zainicjowania `DbContext` używającą `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="1b550-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="1b550-128">Takie podejście nie nadają się do testowania, chyba, że testy target pełnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="1b550-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="1b550-129">Przy użyciu obiektu DbContext z iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="1b550-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="1b550-130">EF Core obsługuje korzystanie z `DbContext` z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="1b550-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="1b550-131">Danego typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>` metody.</span><span class="sxs-lookup"><span data-stu-id="1b550-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="1b550-132">`AddDbContext<TContext>` spowoduje, że oba danego typu DbContext `TContext`i odpowiadający mu `DbContextOptions<TContext>` dostępne iniekcji z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="1b550-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="1b550-133">Zobacz [więcej odczytu](#more-reading) poniżej dodatkowe informacje na temat iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="1b550-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="1b550-134">Dodawanie `Dbcontext` do iniekcji zależności:</span><span class="sxs-lookup"><span data-stu-id="1b550-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="1b550-135">Wymaga to dodawanie [argument konstruktora](#constructor-argument) Twojego typ DbContext, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1b550-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="1b550-136">Kontekst kodu:</span><span class="sxs-lookup"><span data-stu-id="1b550-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="1b550-137">Kod aplikacji (w ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="1b550-137">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="1b550-138">Kod aplikacji (przy użyciu element ServiceProvider bezpośrednio, mniej typowe):</span><span class="sxs-lookup"><span data-stu-id="1b550-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="1b550-139">Więcej odczytu</span><span class="sxs-lookup"><span data-stu-id="1b550-139">More reading</span></span>

* <span data-ttu-id="1b550-140">Odczyt [wprowadzenie na platformy ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji o używaniu EF z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1b550-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="1b550-141">Odczyt [iniekcji zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Aby dowiedzieć się więcej o korzystaniu z Podpisane.</span><span class="sxs-lookup"><span data-stu-id="1b550-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="1b550-142">Odczyt [testowanie](testing/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1b550-142">Read [Testing](testing/index.md) for more information.</span></span>
