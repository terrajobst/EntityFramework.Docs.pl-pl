---
title: Konfigurowanie typu DbContext - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 0350b25d0d0efe05df7cb9e93a3f4ae2d864fd63
ms.sourcegitcommit: 47e0a66a136e743a815d099d2bee5f0da1a068c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363940"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="2706a-102">Konfigurowanie typu DbContext</span><span class="sxs-lookup"><span data-stu-id="2706a-102">Configuring a DbContext</span></span>

<span data-ttu-id="2706a-103">W tym artykule przedstawiono podstawowe wzorce dotyczące konfigurowania `DbContext` za pośrednictwem `DbContextOptions` nawiązać połączenia z bazą danych, używając określonego dostawcy programu EF Core i opcjonalnie zachowania.</span><span class="sxs-lookup"><span data-stu-id="2706a-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="2706a-104">Konfiguracja typu DbContext w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="2706a-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="2706a-105">EF Core z czasu projektowania narzędzi, takich jak [migracje](xref:core/managing-schemas/migrations/index) muszą mieć możliwość odnajdywania i utworzyć wystąpienie pracy `DbContext` typu, aby można było zbierać szczegółowe informacje dotyczące typów jednostek i sposobu mapowania ich na schemat bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2706a-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="2706a-106">Ten proces może być automatyczna, tak długo, jak łatwo można utworzyć narzędzie `DbContext` w taki sposób, że zostanie on skonfigurowany podobnie jak może zostać skonfigurowane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="2706a-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="2706a-107">Podczas gdy wszelkie wzorzec, który dostarcza informacje o konfiguracji niezbędne do `DbContext` może pracować w czasie wykonywania, narzędzi, które wymagają przy użyciu `DbContext` w czasie projektowania może pracować tylko z ograniczonej liczby wzorców.</span><span class="sxs-lookup"><span data-stu-id="2706a-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="2706a-108">Są one objęte bardziej szczegółowo w [czasu projektowania, tworzenia kontekstu](xref:core/miscellaneous/cli/dbcontext-creation) sekcji.</span><span class="sxs-lookup"><span data-stu-id="2706a-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="2706a-109">Konfigurowanie DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="2706a-109">Configuring DbContextOptions</span></span>

`DbContext` <span data-ttu-id="2706a-110">musi mieć instancję `DbContextOptions` aby można było wykonać wszelkie prace.</span><span class="sxs-lookup"><span data-stu-id="2706a-110">must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="2706a-111">`DbContextOptions` Wystąpienia niesie ze sobą informacje o konfiguracji takich jak:</span><span class="sxs-lookup"><span data-stu-id="2706a-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="2706a-112">Dostawca bazy danych do użycia, zwykle wybrane przez wywołanie metody, takie jak `UseSqlServer` lub `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="2706a-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="2706a-113">Te metody rozszerzenia wymagają odpowiedni pakiet dostawcy, takich jak `Microsoft.EntityFrameworkCore.SqlServer` lub `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="2706a-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="2706a-114">Te metody są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="2706a-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="2706a-115">Wszelkie wymagane parametry lub identyfikator wystąpienia bazy danych zazwyczaj przekazywany jako argument do podanej powyżej metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="2706a-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="2706a-116">Żadnych selektorów poziom dostawcy, zachowanie opcjonalne, również są zazwyczaj powiązane wewnątrz wywołania metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="2706a-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="2706a-117">Wszelkie ogólne selektorów zachowanie programu EF Core, zwykle połączonymi w łańcuch po lub przed metodą selektor dostawcy</span><span class="sxs-lookup"><span data-stu-id="2706a-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="2706a-118">Poniższy przykład umożliwia skonfigurowanie `DbContextOptions` do używania dostawcy programu SQL Server, połączenie jest zawarty w `connectionString` zmienną, limit czasu polecenia poziom dostawcy i selektor zachowanie programu EF Core, która sprawia, że wszystkie zapytania wykonywane w `DbContext` [śledzenia nie](xref:core/querying/tracking#no-tracking-queries) domyślnie:</span><span class="sxs-lookup"><span data-stu-id="2706a-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="2706a-119">Dostawca selektora metody i innych metod selektor zachowanie wymienione powyżej są metody rozszerzenia na `DbContextOptions` lub klasy opcji właściwe dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="2706a-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="2706a-120">Aby uzyskać dostęp do tych metod rozszerzenia może być konieczne przestrzeń nazw (zazwyczaj `Microsoft.EntityFrameworkCore`) w zakres i obejmują zależności dodatkowych pakietów w projekcie.</span><span class="sxs-lookup"><span data-stu-id="2706a-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="2706a-121">`DbContextOptions` Mogą być dostarczane do `DbContext` przez zastąpienie `OnConfiguring` metody lub zewnętrznie za pośrednictwem argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="2706a-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="2706a-122">Jeśli używane są obie, `OnConfiguring` ostatnio zastosowane i mogą zastąpić opcje przekazana do argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="2706a-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="2706a-123">Argument konstruktora</span><span class="sxs-lookup"><span data-stu-id="2706a-123">Constructor argument</span></span>

<span data-ttu-id="2706a-124">Kontekst kodu za pomocą konstruktora:</span><span class="sxs-lookup"><span data-stu-id="2706a-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="2706a-125">Podstawowy Konstruktor typu DbContext akceptuje także nieogólnego wersję `DbContextOptions`, ale przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji z wieloma typami kontekstu.</span><span class="sxs-lookup"><span data-stu-id="2706a-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="2706a-126">Kod aplikacji można zainicjować na podstawie argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="2706a-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="2706a-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="2706a-127">OnConfiguring</span></span>

<span data-ttu-id="2706a-128">Kontekst kodu za pomocą `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="2706a-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="2706a-129">Kod aplikacji, aby zainicjować `DbContext` , który używa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="2706a-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="2706a-130">To podejście nie jest przystosowany do testowania, chyba że próby docelowe pełnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2706a-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="2706a-131">Przy użyciu iniekcji zależności typu DbContext</span><span class="sxs-lookup"><span data-stu-id="2706a-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="2706a-132">EF Core obsługuje korzystanie z `DbContext` z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="2706a-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="2706a-133">Typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>` metody.</span><span class="sxs-lookup"><span data-stu-id="2706a-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

`AddDbContext<TContext>` <span data-ttu-id="2706a-134">spowoduje, że oba danego typu DbContext `TContext`i odpowiedni `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="2706a-134">will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="2706a-135">Zobacz [więcej odczytu](#more-reading) poniżej dodatkowe informacje na temat iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="2706a-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="2706a-136">Dodawanie `Dbcontext` do wstrzykiwania zależności:</span><span class="sxs-lookup"><span data-stu-id="2706a-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="2706a-137">To wymaga dodania [argumentu konstruktora](#constructor-argument) do danego typu DbContext, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="2706a-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="2706a-138">Kontekst kodu:</span><span class="sxs-lookup"><span data-stu-id="2706a-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="2706a-139">Kod aplikacji (w programie ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="2706a-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="2706a-140">Kod aplikacji (przy użyciu element ServiceProvider bezpośrednio, mniej typowe):</span><span class="sxs-lookup"><span data-stu-id="2706a-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="2706a-141">Unikanie DbContext problemy wielowątkowości</span><span class="sxs-lookup"><span data-stu-id="2706a-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="2706a-142">Entity Framework Core nie obsługuje wielu operacji równoległych, które są uruchamiane na tym samym `DbContext` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="2706a-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="2706a-143">Równoczesny dostęp może spowodować niezdefiniowane zachowanie, awarii aplikacji i uszkodzenie danych.</span><span class="sxs-lookup"><span data-stu-id="2706a-143">Concurrent access can result in undefined behavior, application crashes and data corruption.</span></span> <span data-ttu-id="2706a-144">W związku z tym ważne jest, aby zawsze używać oddzielnych `DbContext` wystąpień dla operacji, które są wykonywane równolegle.</span><span class="sxs-lookup"><span data-stu-id="2706a-144">Because of this it's important to always use separate `DbContext` instances for operations that execute in parallel.</span></span> 

<span data-ttu-id="2706a-145">Istnieją typowych błędów, które można inadvernetly Przyczyna równoczesny dostęp na tym samym `DbContext` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="2706a-145">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="2706a-146">Zapominanie o oczekiwać na zakończenie operacji asynchronicznej przed uruchomieniem innych operacji na tym samym typem DbContext</span><span class="sxs-lookup"><span data-stu-id="2706a-146">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="2706a-147">Metody asynchroniczne umożliwiają programu EF Core można zainicjować operacji, które dostęp do bazy danych w taki sposób, bez blokowania.</span><span class="sxs-lookup"><span data-stu-id="2706a-147">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="2706a-148">Ale jeśli obiekt wywołujący nie oczekiwać na zakończenie jednego z tych metod, a następnie przechodzi do wykonywania innych operacji na `DbContext`, stan `DbContext` może być (i będzie bardzo prawdopodobne) uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="2706a-148">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="2706a-149">Zawsze czekać natychmiast metod asynchronicznych programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="2706a-149">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="2706a-150">Niejawnie udostępnianie wystąpień typu DbContext między wieloma wątkami za pomocą iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="2706a-150">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="2706a-151">[ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Rejestruje metody rozszerzenia `DbContext` typów z [o określonym zakresie okres istnienia](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) domyślnie.</span><span class="sxs-lookup"><span data-stu-id="2706a-151">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="2706a-152">Jest to pozbawione równoczesny dostęp problemy w aplikacji platformy ASP.NET Core, ponieważ istnieje tylko jeden wątek wykonywania każdego żądania klienta w danym momencie, a każde żądanie pobiera zakres iniekcji zależności oddzielne (i w związku z tym odrębne `DbContext` wystąpienie).</span><span class="sxs-lookup"><span data-stu-id="2706a-152">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="2706a-153">Jednak każdy kod, który jawnie wykonuje wiele wątków w paralell upewnij się, że `DbContext` wystąpienia nie są nigdy nie accesed jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="2706a-153">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="2706a-154">Przy użyciu iniekcji zależności, można to osiągnąć, rejestrując kontekst jako zakresie oraz tworzenie zakresów (przy użyciu `IServiceScopeFactory`) dla każdego wątku lub po zarejestrowaniu `DbContext` jako przejściowe (za pomocą przeciążenia `AddDbContext` używającą `ServiceLifetime` parametru).</span><span class="sxs-lookup"><span data-stu-id="2706a-154">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="2706a-155">Odczytywanie więcej</span><span class="sxs-lookup"><span data-stu-id="2706a-155">More reading</span></span>

* <span data-ttu-id="2706a-156">Odczyt [wprowadzenie do programu ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji na temat korzystania z programów EF z platformą ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2706a-156">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="2706a-157">Odczyt [wstrzykiwanie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Aby dowiedzieć się więcej o korzystaniu z DI.</span><span class="sxs-lookup"><span data-stu-id="2706a-157">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="2706a-158">Odczyt [testowania](testing/index.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="2706a-158">Read [Testing](testing/index.md) for more information.</span></span>
