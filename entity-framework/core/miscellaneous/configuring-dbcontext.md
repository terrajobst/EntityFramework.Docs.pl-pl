---
title: Konfigurowanie DbContext - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="4f77b-102">Konfigurowanie obiektu DbContext</span><span class="sxs-lookup"><span data-stu-id="4f77b-102">Configuring a DbContext</span></span>

<span data-ttu-id="4f77b-103">W tym artykule przedstawiono wzorce konfigurowania `DbContext` z `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4f77b-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="4f77b-104">Opcje są używane głównie aby wybrać i skonfigurować Magazyn danych.</span><span class="sxs-lookup"><span data-stu-id="4f77b-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="4f77b-105">Konfigurowanie DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="4f77b-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="4f77b-106">`DbContext`musi być wystąpieniem `DbContextOptions` w celu wykonania.</span><span class="sxs-lookup"><span data-stu-id="4f77b-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="4f77b-107">Tę funkcję można skonfigurować przez zastąpienie `OnConfiguring`, lub zewnętrznie dostarczony przez argument konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4f77b-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="4f77b-108">Jeśli są używane, `OnConfiguring` jest wykonywana na podane opcje, co oznacza jest dodatek i może zastąpić opcje przekazany do argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4f77b-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="4f77b-109">Argument konstruktora</span><span class="sxs-lookup"><span data-stu-id="4f77b-109">Constructor argument</span></span>

<span data-ttu-id="4f77b-110">Kontekst kodu za pomocą konstruktora</span><span class="sxs-lookup"><span data-stu-id="4f77b-110">Context code with constructor</span></span>

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
> <span data-ttu-id="4f77b-111">Podstawowy Konstruktor DbContext również zaakceptowanie wersji nieogólnego `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4f77b-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="4f77b-112">Przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji o wiele typów kontekstu.</span><span class="sxs-lookup"><span data-stu-id="4f77b-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="4f77b-113">Kod aplikacji, można zainicjować na podstawie argumentów konstruktora</span><span class="sxs-lookup"><span data-stu-id="4f77b-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="4f77b-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="4f77b-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="4f77b-115">`OnConfiguring`Ostatnia i mogą zastąpić opcje uzyskane z Podpisane lub konstruktora.</span><span class="sxs-lookup"><span data-stu-id="4f77b-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="4f77b-116">Takie podejście nie nadają się do testowania (chyba że zostanie rozpoczęta dla pełnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="4f77b-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="4f77b-117">Kontekst kodu za pomocą `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4f77b-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="4f77b-118">Kod aplikacji, aby zainicjować z `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="4f77b-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="4f77b-119">Przy użyciu obiektu DbContext z iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="4f77b-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="4f77b-120">Obsługa EF przy użyciu `DbContext` z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="4f77b-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="4f77b-121">Danego typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="4f77b-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="4f77b-122">`AddDbContext`spowoduje, że oba danego typu DbContext `TContext`, i `DbContextOptions<TContext>` dostępne iniekcji z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="4f77b-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="4f77b-123">Zobacz [więcej odczytu](#more-reading) poniżej informacje na iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="4f77b-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="4f77b-124">Dodawanie dbcontext do iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="4f77b-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="4f77b-125">Wymaga to dodawanie [argument konstruktora](#constructor-argument) Twojego typ DbContext, który akceptuje `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="4f77b-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="4f77b-126">Kontekst kodu:</span><span class="sxs-lookup"><span data-stu-id="4f77b-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="4f77b-127">Kod aplikacji (w ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="4f77b-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="4f77b-128">Kod aplikacji (przy użyciu element ServiceProvider bezpośrednio, mniej typowe):</span><span class="sxs-lookup"><span data-stu-id="4f77b-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="4f77b-129">Za pomocą`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="4f77b-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="4f77b-130">Alternatywą dla powyższych opcji może również zapewniać implementację elementu `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="4f77b-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="4f77b-131">Narzędzia EF umożliwia utworzenie wystąpienia DbContext z tej fabryki.</span><span class="sxs-lookup"><span data-stu-id="4f77b-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="4f77b-132">Może to być wymagane w celu umożliwienia określonego środowiska czasu projektowania, takich jak migracji.</span><span class="sxs-lookup"><span data-stu-id="4f77b-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="4f77b-133">Implementuje ten interfejs, aby włączyć usługi czasu projektowania dla typów kontekstu, które nie ma publicznego konstruktora domyślnego.</span><span class="sxs-lookup"><span data-stu-id="4f77b-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="4f77b-134">Usługi w czasie projektowania automatycznie wykryje implementacje tego interfejsu, które znajdują się w tym samym zestawie co kontekst pochodnych.</span><span class="sxs-lookup"><span data-stu-id="4f77b-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="4f77b-135">Przykład:</span><span class="sxs-lookup"><span data-stu-id="4f77b-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="4f77b-136">Więcej odczytu</span><span class="sxs-lookup"><span data-stu-id="4f77b-136">More reading</span></span>

* <span data-ttu-id="4f77b-137">Odczyt [wprowadzenie na platformy ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji o używaniu EF z platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f77b-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="4f77b-138">Odczyt [iniekcji zależności](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) Aby dowiedzieć się więcej o korzystaniu z Podpisane.</span><span class="sxs-lookup"><span data-stu-id="4f77b-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="4f77b-139">Odczyt [testowanie](testing/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="4f77b-139">Read [Testing](testing/index.md) for more information.</span></span>
