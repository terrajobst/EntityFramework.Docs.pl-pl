---
title: Konfigurowanie DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 734acad86e364abbfd1522fe79d4a847b1acfb52
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149039"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="c7c92-102">Konfigurowanie DbContext</span><span class="sxs-lookup"><span data-stu-id="c7c92-102">Configuring a DbContext</span></span>

<span data-ttu-id="c7c92-103">W tym artykule przedstawiono podstawowe wzorce konfigurowania programu `DbContext` `DbContextOptions` za pośrednictwem programu w celu nawiązania połączenia z bazą danych przy użyciu określonego dostawcy EF Core i opcjonalnych zachowań.</span><span class="sxs-lookup"><span data-stu-id="c7c92-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="c7c92-104">Konfiguracja DbContext czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="c7c92-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="c7c92-105">EF Core narzędzia czasu projektowania, takie jak [migracje](xref:core/managing-schemas/migrations/index) , muszą mieć możliwość odnajdywania i tworzenia działającego wystąpienia `DbContext` typu w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c7c92-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c7c92-106">Ten proces może być automatyczny, tak długo, jak narzędzie można łatwo utworzyć `DbContext` w taki sposób, że zostanie ono skonfigurowane podobnie do konfiguracji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c7c92-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="c7c92-107">Chociaż każdy wzorzec, który zapewnia niezbędne informacje `DbContext` konfiguracyjne, może działać w czasie wykonywania, narzędzia, które wymagają `DbContext` użycia w czasie projektowania, mogą pracować tylko z ograniczoną liczbą wzorców.</span><span class="sxs-lookup"><span data-stu-id="c7c92-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="c7c92-108">Są one omówione bardziej szczegółowo w sekcji [Tworzenie kontekstu w czasie projektowania](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="c7c92-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="c7c92-109">Konfigurowanie DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="c7c92-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="c7c92-110">`DbContext`musi mieć wystąpienie, `DbContextOptions` aby można było wykonać dowolną ilość pracy.</span><span class="sxs-lookup"><span data-stu-id="c7c92-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="c7c92-111">`DbContextOptions` Wystąpienie przenosi informacje o konfiguracji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="c7c92-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="c7c92-112">Dostawca bazy danych do użycia, zazwyczaj wybierany przez wywołanie metody, takiej jak `UseSqlServer` lub `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="c7c92-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="c7c92-113">Te metody rozszerzające wymagają odpowiedniego pakietu dostawcy, takiego jak `Microsoft.EntityFrameworkCore.SqlServer` lub `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="c7c92-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="c7c92-114">Metody są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c7c92-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="c7c92-115">Wszystkie niezbędne parametry połączenia lub identyfikator wystąpienia bazy danych, zwykle przesyłane jako argument do metody wyboru dostawcy wymienione powyżej</span><span class="sxs-lookup"><span data-stu-id="c7c92-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="c7c92-116">Wszystkie selektory nieopcjonalnych zachowań na poziomie dostawcy, zwykle również łańcucha wewnątrz wywołania metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="c7c92-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="c7c92-117">Wszystkie selektory zachowań ogólnych EF Core, zwykle łańcucha po lub przed metodą selektora dostawcy</span><span class="sxs-lookup"><span data-stu-id="c7c92-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="c7c92-118">Poniższy przykład konfiguruje `DbContextOptions` używanie dostawcy SQL Server, połączenie zawarte `connectionString` w zmiennej, przekroczenie limitu czasu polecenia dostawcy i selektor zachowania EF Core, które wykonuje `DbContext` wszystkie zapytania wykonywane w Domyślnie [nie śledzone](xref:core/querying/tracking#no-tracking-queries) :</span><span class="sxs-lookup"><span data-stu-id="c7c92-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="c7c92-119">Metody selektora dostawcy i inne metody selektora zachowań wymienione powyżej to metody `DbContextOptions` rozszerzające dotyczące klas opcji lub specyficznych dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="c7c92-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="c7c92-120">Aby można było uzyskać dostęp do tych metod rozszerzających, może być konieczne posiadanie przestrzeni nazw ( `Microsoft.EntityFrameworkCore`zazwyczaj) w zakresie i uwzględnienie dodatkowych zależności pakietu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="c7c92-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="c7c92-121">Można dostarczyć do obiektu, zastępując `OnConfiguring` metodę lub zewnętrznie za pośrednictwem argumentu konstruktora. `DbContext` `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="c7c92-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="c7c92-122">Jeśli oba są używane, `OnConfiguring` są stosowane jako ostatnie i mogą zastąpić opcje dostarczone do argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="c7c92-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="c7c92-123">Argument konstruktora</span><span class="sxs-lookup"><span data-stu-id="c7c92-123">Constructor argument</span></span>

<span data-ttu-id="c7c92-124">Kod kontekstu z konstruktorem:</span><span class="sxs-lookup"><span data-stu-id="c7c92-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="c7c92-125">Konstruktor podstawowy DbContext również akceptuje nieogólną wersję `DbContextOptions`, ale użycie nieogólnej wersji nie jest zalecane w przypadku aplikacji z wieloma typami kontekstu.</span><span class="sxs-lookup"><span data-stu-id="c7c92-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="c7c92-126">Kod aplikacji do zainicjowania z argumentu konstruktora:</span><span class="sxs-lookup"><span data-stu-id="c7c92-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="c7c92-127">Trwa konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="c7c92-127">OnConfiguring</span></span>

<span data-ttu-id="c7c92-128">Kod kontekstu z `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="c7c92-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="c7c92-129">Kod aplikacji do zainicjowania `DbContext` , który `OnConfiguring`używa:</span><span class="sxs-lookup"><span data-stu-id="c7c92-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="c7c92-130">Takie podejście nie nadaje się do testowania, chyba że testy są przeznaczone dla całej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c7c92-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="c7c92-131">Używanie DbContext z iniekcją zależności</span><span class="sxs-lookup"><span data-stu-id="c7c92-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="c7c92-132">EF Core obsługuje używanie `DbContext` z kontenerem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="c7c92-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="c7c92-133">Typ kontekstu DbContext można dodać do kontenera usługi przy użyciu `AddDbContext<TContext>` metody.</span><span class="sxs-lookup"><span data-stu-id="c7c92-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="c7c92-134">`AddDbContext<TContext>`nastąpi zarówno typ kontekstu DB, `TContext`jak i odpowiednie `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="c7c92-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="c7c92-135">Aby uzyskać dodatkowe informacje na temat iniekcji zależności, zobacz [więcej](#more-reading) informacji poniżej.</span><span class="sxs-lookup"><span data-stu-id="c7c92-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="c7c92-136">`DbContext` Dodawanie do iniekcji zależności:</span><span class="sxs-lookup"><span data-stu-id="c7c92-136">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="c7c92-137">Wymaga to dodania [argumentu konstruktora](#constructor-argument) do typu DbContext, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c7c92-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="c7c92-138">Kod kontekstu:</span><span class="sxs-lookup"><span data-stu-id="c7c92-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="c7c92-139">Kod aplikacji (w ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="c7c92-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="c7c92-140">Kod aplikacji (bezpośrednio, rzadziej używany):</span><span class="sxs-lookup"><span data-stu-id="c7c92-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="c7c92-141">Unikanie problemów wielowątkowości DbContext</span><span class="sxs-lookup"><span data-stu-id="c7c92-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="c7c92-142">Entity Framework Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym `DbContext` wystąpieniu.</span><span class="sxs-lookup"><span data-stu-id="c7c92-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="c7c92-143">Obejmuje to zarówno równoległe wykonywanie zapytań asynchronicznych, jak i jawne jednoczesne użycie z wielu wątków.</span><span class="sxs-lookup"><span data-stu-id="c7c92-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="c7c92-144">W związku z `await` tym zawsze asynchroniczne wywołania są natychmiast lub `DbContext` używają osobnych wystąpień dla operacji wykonywanych równolegle.</span><span class="sxs-lookup"><span data-stu-id="c7c92-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="c7c92-145">Gdy EF Core wykryje próbę użycia `DbContext` wystąpienia współbieżnie, `InvalidOperationException` zobaczysz komunikat o takiej postaci:</span><span class="sxs-lookup"><span data-stu-id="c7c92-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="c7c92-146">Druga operacja rozpoczęta w tym kontekście przed ukończeniem poprzedniej operacji.</span><span class="sxs-lookup"><span data-stu-id="c7c92-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="c7c92-147">Jest to zazwyczaj spowodowane przez różne wątki korzystające z tego samego wystąpienia DbContext, jednak elementy członkowskie wystąpienia nie są gwarantowane bezpieczny wątkowo.</span><span class="sxs-lookup"><span data-stu-id="c7c92-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="c7c92-148">Gdy dostęp współbieżny nie zostanie wykryty, może to spowodować niezdefiniowane zachowanie, awarie aplikacji i uszkodzenie danych.</span><span class="sxs-lookup"><span data-stu-id="c7c92-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="c7c92-149">Istnieją typowe błędy, które mogą przypadkowo spowodować jednoczesne dostęp do tego samego `DbContext` wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="c7c92-149">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="c7c92-150">Zapominanie do oczekiwania na ukończenie operacji asynchronicznej przed rozpoczęciem jakiejkolwiek innej operacji w tym samym kontekście DbContext</span><span class="sxs-lookup"><span data-stu-id="c7c92-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="c7c92-151">Metody asynchroniczne umożliwiają EF Core inicjowania operacji, które uzyskują dostęp do bazy danych w sposób nieblokowany.</span><span class="sxs-lookup"><span data-stu-id="c7c92-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="c7c92-152">Ale jeśli obiekt wywołujący nie oczekuje na zakończenie jednej z tych metod, a następnie wykonuje inne operacje na `DbContext`, stan `DbContext` może być (i bardzo prawdopodobnie będzie) uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="c7c92-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="c7c92-153">Zawsze Czekaj na EF Core metod asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="c7c92-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="c7c92-154">Niejawnie udostępnianie wystąpień DbContext w wielu wątkach przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="c7c92-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="c7c92-155">Metoda rozszerzająca domyślnie `DbContext` rejestruje typy z [okresem istnienia w zakresie.](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)</span><span class="sxs-lookup"><span data-stu-id="c7c92-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="c7c92-156">Jest to bezpieczne z równoczesnych problemów z dostępem w aplikacjach ASP.NET Core, ponieważ w danym momencie istnieje tylko jeden wątek wykonujący każde żądanie klienta, a ponieważ każde żądanie pobiera oddzielny zakres iniekcji zależności ( `DbContext` i w związku z tym oddzielny wystąpienie).</span><span class="sxs-lookup"><span data-stu-id="c7c92-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="c7c92-157">Jednak każdy kod, który jawnie wykonuje wiele wątków równolegle, powinien mieć `DbContext` pewność, że wystąpienia nie są kiedykolwiek dostępne współbieżnie.</span><span class="sxs-lookup"><span data-stu-id="c7c92-157">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="c7c92-158">Przy użyciu iniekcji zależności można to osiągnąć przez zarejestrowanie kontekstu jako zakresu i Tworzenie zakresów (za pomocą `IServiceScopeFactory`) dla każdego wątku lub przez `DbContext` zarejestrowanie jako przejściowego `AddDbContext` (przy użyciu przeciążenia, które przyjmuje `ServiceLifetime` parametr).</span><span class="sxs-lookup"><span data-stu-id="c7c92-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="c7c92-159">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="c7c92-159">More reading</span></span>

* <span data-ttu-id="c7c92-160">Przeczytaj [wstrzyknięcie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , aby dowiedzieć się więcej o używaniu di.</span><span class="sxs-lookup"><span data-stu-id="c7c92-160">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="c7c92-161">Przeczytaj [test](testing/index.md) , aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c7c92-161">Read [Testing](testing/index.md) for more information.</span></span>
