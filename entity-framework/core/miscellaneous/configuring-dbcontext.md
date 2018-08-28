---
title: Konfigurowanie typu DbContext - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995941"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="25a1f-102">Konfigurowanie typu DbContext</span><span class="sxs-lookup"><span data-stu-id="25a1f-102">Configuring a DbContext</span></span>

<span data-ttu-id="25a1f-103">W tym artykule przedstawiono podstawowe wzorce dotyczące konfigurowania `DbContext` za pośrednictwem `DbContextOptions` nawiązać połączenia z bazą danych, używając określonego dostawcy programu EF Core i opcjonalnie zachowania.</span><span class="sxs-lookup"><span data-stu-id="25a1f-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="25a1f-104">Konfiguracja typu DbContext w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="25a1f-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="25a1f-105">EF Core z czasu projektowania narzędzi, takich jak [migracje](xref:core/managing-schemas/migrations/index) muszą mieć możliwość odnajdywania i utworzyć wystąpienie pracy `DbContext` typu, aby można było zbierać szczegółowe informacje dotyczące typów jednostek i sposobu mapowania ich na schemat bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="25a1f-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="25a1f-106">Ten proces może być automatyczna, tak długo, jak łatwo można utworzyć narzędzie `DbContext` w taki sposób, że zostanie on skonfigurowany podobnie jak może zostać skonfigurowane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="25a1f-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="25a1f-107">Podczas gdy wszelkie wzorzec, który dostarcza informacje o konfiguracji niezbędne do `DbContext` może pracować w czasie wykonywania, narzędzi, które wymagają przy użyciu `DbContext` w czasie projektowania może pracować tylko z ograniczonej liczby wzorców.</span><span class="sxs-lookup"><span data-stu-id="25a1f-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="25a1f-108">Są one objęte bardziej szczegółowo w [czasu projektowania, tworzenia kontekstu](xref:core/miscellaneous/cli/dbcontext-creation) sekcji.</span><span class="sxs-lookup"><span data-stu-id="25a1f-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="25a1f-109">Konfigurowanie DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="25a1f-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="25a1f-110">`DbContext` musi mieć instancję `DbContextOptions` aby można było wykonać wszelkie prace.</span><span class="sxs-lookup"><span data-stu-id="25a1f-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="25a1f-111">`DbContextOptions` Wystąpienia niesie ze sobą informacje o konfiguracji takich jak:</span><span class="sxs-lookup"><span data-stu-id="25a1f-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="25a1f-112">Dostawca bazy danych do użycia, zwykle wybrane przez wywołanie metody, takie jak `UseSqlServer` lub `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="25a1f-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="25a1f-113">Wszelkie wymagane parametry lub identyfikator wystąpienia bazy danych zazwyczaj przekazywany jako argument do podanej powyżej metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="25a1f-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="25a1f-114">Żadnych selektorów poziom dostawcy, zachowanie opcjonalne, również są zazwyczaj powiązane wewnątrz wywołania metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="25a1f-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="25a1f-115">Wszelkie ogólne selektorów zachowanie programu EF Core, zwykle połączonymi w łańcuch po lub przed metodą selektor dostawcy</span><span class="sxs-lookup"><span data-stu-id="25a1f-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="25a1f-116">Poniższy przykład umożliwia skonfigurowanie `DbContextOptions` do używania dostawcy programu SQL Server, połączenie jest zawarty w `connectionString` zmienną, limit czasu polecenia poziom dostawcy i selektor zachowanie programu EF Core, która sprawia, że wszystkie zapytania wykonywane w `DbContext` [śledzenia nie](xref:core/querying/tracking#no-tracking-queries) domyślnie:</span><span class="sxs-lookup"><span data-stu-id="25a1f-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="25a1f-117">Dostawca selektora metody i innych metod selektor zachowanie wymienione powyżej są metody rozszerzenia na `DbContextOptions` lub klasy opcji właściwe dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="25a1f-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="25a1f-118">Aby uzyskać dostęp do tych metod rozszerzenia może być konieczne przestrzeń nazw (zazwyczaj `Microsoft.EntityFrameworkCore`) w zakres i obejmują zależności dodatkowych pakietów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="25a1f-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="25a1f-119">`DbContextOptions` Mogą być dostarczane do `DbContext` przez zastąpienie `OnConfiguring` metody lub zewnętrznie za pośrednictwem argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="25a1f-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="25a1f-120">Jeśli używane są obie, `OnConfiguring` ostatnio zastosowane i mogą zastąpić opcje przekazana do argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="25a1f-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="25a1f-121">Argument konstruktora</span><span class="sxs-lookup"><span data-stu-id="25a1f-121">Constructor argument</span></span>

<span data-ttu-id="25a1f-122">Kontekst kodu za pomocą konstruktora:</span><span class="sxs-lookup"><span data-stu-id="25a1f-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="25a1f-123">Podstawowy Konstruktor typu DbContext akceptuje także nieogólnego wersję `DbContextOptions`, ale przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji z wieloma typami kontekstu.</span><span class="sxs-lookup"><span data-stu-id="25a1f-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="25a1f-124">Kod aplikacji można zainicjować na podstawie argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="25a1f-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="25a1f-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="25a1f-125">OnConfiguring</span></span>

<span data-ttu-id="25a1f-126">Kontekst kodu za pomocą `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="25a1f-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="25a1f-127">Kod aplikacji, aby zainicjować `DbContext` , który używa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="25a1f-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="25a1f-128">To podejście nie jest przystosowany do testowania, chyba że próby docelowe pełnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="25a1f-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="25a1f-129">Przy użyciu iniekcji zależności typu DbContext</span><span class="sxs-lookup"><span data-stu-id="25a1f-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="25a1f-130">EF Core obsługuje korzystanie z `DbContext` z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="25a1f-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="25a1f-131">Typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>` metody.</span><span class="sxs-lookup"><span data-stu-id="25a1f-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="25a1f-132">`AddDbContext<TContext>` spowoduje, że oba danego typu DbContext `TContext`i odpowiedni `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="25a1f-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="25a1f-133">Zobacz [więcej odczytu](#more-reading) poniżej dodatkowe informacje na temat iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="25a1f-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="25a1f-134">Dodawanie `Dbcontext` do wstrzykiwania zależności:</span><span class="sxs-lookup"><span data-stu-id="25a1f-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="25a1f-135">To wymaga dodania [argumentu konstruktora](#constructor-argument) do danego typu DbContext, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="25a1f-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="25a1f-136">Kontekst kodu:</span><span class="sxs-lookup"><span data-stu-id="25a1f-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="25a1f-137">Kod aplikacji (w programie ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="25a1f-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="25a1f-138">Kod aplikacji (przy użyciu element ServiceProvider bezpośrednio, mniej typowe):</span><span class="sxs-lookup"><span data-stu-id="25a1f-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="25a1f-139">Odczytywanie więcej</span><span class="sxs-lookup"><span data-stu-id="25a1f-139">More reading</span></span>

* <span data-ttu-id="25a1f-140">Odczyt [wprowadzenie do programu ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji na temat korzystania z programów EF z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25a1f-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="25a1f-141">Odczyt [wstrzykiwanie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Aby dowiedzieć się więcej o korzystaniu z DI.</span><span class="sxs-lookup"><span data-stu-id="25a1f-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="25a1f-142">Odczyt [testowania](testing/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="25a1f-142">Read [Testing](testing/index.md) for more information.</span></span>
