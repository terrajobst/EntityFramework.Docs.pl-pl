---
title: Konfigurowanie DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 53b38f288cd45e66d68ebcc3b6066646d59b0262
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136189"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="10dc3-102">Konfigurowanie klasy DbContext</span><span class="sxs-lookup"><span data-stu-id="10dc3-102">Configuring a DbContext</span></span>

<span data-ttu-id="10dc3-103">W tym artykule przedstawiono podstawowe wzorce konfigurowania `DbContext` za pośrednictwem `DbContextOptions` do łączenia się z bazą danych przy użyciu określonego dostawcy EF Core i opcjonalnych zachowań.</span><span class="sxs-lookup"><span data-stu-id="10dc3-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="10dc3-104">Konfiguracja DbContext czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="10dc3-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="10dc3-105">EF Core narzędzia czasu projektowania, takie jak [migracje](xref:core/managing-schemas/migrations/index) , muszą mieć możliwość odnajdywania i tworzenia działającego wystąpienia typu `DbContext` w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="10dc3-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="10dc3-106">Ten proces może być automatyczny, o ile narzędzie może łatwo utworzyć `DbContext` w taki sposób, że zostanie on skonfigurowany podobnie do konfiguracji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="10dc3-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="10dc3-107">Chociaż każdy wzorzec zapewniający niezbędne informacje konfiguracyjne dla `DbContext` może działać w czasie wykonywania, narzędzia, które wymagają użycia `DbContext` w czasie projektowania, mogą pracować tylko z ograniczoną liczbą wzorców.</span><span class="sxs-lookup"><span data-stu-id="10dc3-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="10dc3-108">Są one omówione bardziej szczegółowo w sekcji [Tworzenie kontekstu w czasie projektowania](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="10dc3-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="10dc3-109">Konfigurowanie DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="10dc3-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="10dc3-110">`DbContext` musi mieć wystąpienie `DbContextOptions`, aby można było wykonać dowolną ilość pracy.</span><span class="sxs-lookup"><span data-stu-id="10dc3-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="10dc3-111">Wystąpienie `DbContextOptions` przenosi informacje o konfiguracji, takie jak:</span><span class="sxs-lookup"><span data-stu-id="10dc3-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="10dc3-112">Dostawca bazy danych do użycia, zazwyczaj wybierany przez wywołanie metody, takiej jak `UseSqlServer` lub `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="10dc3-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="10dc3-113">Te metody rozszerzające wymagają odpowiedniego pakietu dostawcy, takiego jak `Microsoft.EntityFrameworkCore.SqlServer` lub `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="10dc3-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="10dc3-114">Metody są zdefiniowane w przestrzeni nazw `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="10dc3-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="10dc3-115">Wszystkie niezbędne parametry połączenia lub identyfikator wystąpienia bazy danych, zwykle przesyłane jako argument do metody wyboru dostawcy wymienione powyżej</span><span class="sxs-lookup"><span data-stu-id="10dc3-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="10dc3-116">Wszystkie selektory nieopcjonalnych zachowań na poziomie dostawcy, zwykle również łańcucha wewnątrz wywołania metody wyboru dostawcy</span><span class="sxs-lookup"><span data-stu-id="10dc3-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="10dc3-117">Wszystkie selektory zachowań ogólnych EF Core, zwykle łańcucha po lub przed metodą selektora dostawcy</span><span class="sxs-lookup"><span data-stu-id="10dc3-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="10dc3-118">Poniższy przykład konfiguruje `DbContextOptions` do korzystania z dostawcy SQL Server, połączenie zawarte w zmiennej `connectionString`, przekroczenie limitu czasu polecenia dostawcy i selektor zachowania EF Core, które domyślnie wykonuje wszystkie zapytania wykonywane w `DbContext` [bez śledzenia](xref:core/querying/tracking#no-tracking-queries) :</span><span class="sxs-lookup"><span data-stu-id="10dc3-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="10dc3-119">Metody selektora dostawcy i inne metody selektora zachowań wymienione powyżej to metody rozszerzające dotyczące `DbContextOptions` lub klas opcji specyficznych dla dostawcy.</span><span class="sxs-lookup"><span data-stu-id="10dc3-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="10dc3-120">Aby mieć dostęp do tych metod rozszerzających, może być konieczne posiadanie przestrzeni nazw (zwykle `Microsoft.EntityFrameworkCore`) w zakresie i uwzględnienie w projekcie dodatkowych zależności pakietów.</span><span class="sxs-lookup"><span data-stu-id="10dc3-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="10dc3-121">`DbContextOptions` można dostarczyć do `DbContext` przez zastąpienie metody `OnConfiguring` lub zewnętrznie za pośrednictwem argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="10dc3-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="10dc3-122">Jeśli oba są używane, `OnConfiguring` jest stosowany jako ostatni i mogą zastąpić opcje dostarczone do argumentu konstruktora.</span><span class="sxs-lookup"><span data-stu-id="10dc3-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="10dc3-123">Argument konstruktora</span><span class="sxs-lookup"><span data-stu-id="10dc3-123">Constructor argument</span></span>

<span data-ttu-id="10dc3-124">Konstruktor może po prostu zaakceptować `DbContextOptions` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="10dc3-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

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
> <span data-ttu-id="10dc3-125">Konstruktor podstawowy DbContext również akceptuje nieogólną wersję `DbContextOptions`, ale użycie nieogólnej wersji nie jest zalecane w przypadku aplikacji z wieloma typami kontekstu.</span><span class="sxs-lookup"><span data-stu-id="10dc3-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="10dc3-126">Aplikacja może teraz przekazać `DbContextOptions` podczas tworzenia wystąpienia kontekstu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="10dc3-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="10dc3-127">Trwa konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="10dc3-127">OnConfiguring</span></span>

<span data-ttu-id="10dc3-128">Możesz również zainicjować `DbContextOptions` w samym kontekście.</span><span class="sxs-lookup"><span data-stu-id="10dc3-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="10dc3-129">Chociaż ta technika jest używana w konfiguracji podstawowej, zwykle nadal trzeba uzyskać pewne szczegóły konfiguracji z zewnątrz, np. parametry połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="10dc3-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="10dc3-130">Można to zrobić za pomocą interfejsu API konfiguracji lub w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="10dc3-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="10dc3-131">Aby zainicjować `DbContextOptions` w kontekście, Zastąp metodę `OnConfiguring` i wywołaj metody z podanej `DbContextOptionsBuilder`:</span><span class="sxs-lookup"><span data-stu-id="10dc3-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

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

<span data-ttu-id="10dc3-132">Aplikacja może po prostu utworzyć wystąpienie takiego kontekstu bez przekazywania wszystkiego do jego konstruktora:</span><span class="sxs-lookup"><span data-stu-id="10dc3-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="10dc3-133">Takie podejście nie nadaje się do testowania, chyba że testy są przeznaczone dla całej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="10dc3-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="10dc3-134">Używanie DbContext z iniekcją zależności</span><span class="sxs-lookup"><span data-stu-id="10dc3-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="10dc3-135">EF Core obsługuje używanie `DbContext` z kontenerem iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="10dc3-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="10dc3-136">Typ kontekstu DbContext można dodać do kontenera usługi za pomocą metody `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="10dc3-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="10dc3-137">`AddDbContext<TContext>` spowoduje, że zarówno typ kontekstu DB, `TContext`, jak i odpowiednie `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="10dc3-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="10dc3-138">Aby uzyskać dodatkowe informacje na temat iniekcji zależności, zobacz [więcej](#more-reading) informacji poniżej.</span><span class="sxs-lookup"><span data-stu-id="10dc3-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="10dc3-139">Dodawanie `DbContext` do iniekcji zależności:</span><span class="sxs-lookup"><span data-stu-id="10dc3-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="10dc3-140">Wymaga to dodania [argumentu konstruktora](#constructor-argument) do typu DbContext, który akceptuje `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="10dc3-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="10dc3-141">Kod kontekstu:</span><span class="sxs-lookup"><span data-stu-id="10dc3-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="10dc3-142">Kod aplikacji (w ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="10dc3-142">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="10dc3-143">Kod aplikacji (bezpośrednio, rzadziej używany):</span><span class="sxs-lookup"><span data-stu-id="10dc3-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="10dc3-144">Unikanie problemów wielowątkowości DbContext</span><span class="sxs-lookup"><span data-stu-id="10dc3-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="10dc3-145">Entity Framework Core nie obsługuje wielu operacji równoległych dla tego samego wystąpienia `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="10dc3-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="10dc3-146">Obejmuje to zarówno równoległe wykonywanie zapytań asynchronicznych, jak i jawne jednoczesne użycie z wielu wątków.</span><span class="sxs-lookup"><span data-stu-id="10dc3-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="10dc3-147">W związku z tym zawsze należy natychmiast `await` asynchroniczne wywołania lub używać oddzielnych wystąpień `DbContext` dla operacji wykonywanych równolegle.</span><span class="sxs-lookup"><span data-stu-id="10dc3-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="10dc3-148">Gdy EF Core wykryje próbę użycia wystąpienia `DbContext` współbieżnie, zobaczysz `InvalidOperationException` z komunikatem podobnym do tego:</span><span class="sxs-lookup"><span data-stu-id="10dc3-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="10dc3-149">Druga operacja rozpoczęta w tym kontekście przed ukończeniem poprzedniej operacji.</span><span class="sxs-lookup"><span data-stu-id="10dc3-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="10dc3-150">Jest to zazwyczaj spowodowane przez różne wątki korzystające z tego samego wystąpienia DbContext, jednak elementy członkowskie wystąpienia nie są gwarantowane bezpieczny wątkowo.</span><span class="sxs-lookup"><span data-stu-id="10dc3-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="10dc3-151">Gdy dostęp współbieżny nie zostanie wykryty, może to spowodować niezdefiniowane zachowanie, awarie aplikacji i uszkodzenie danych.</span><span class="sxs-lookup"><span data-stu-id="10dc3-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="10dc3-152">Istnieją typowe błędy, które mogą przypadkowo spowodować jednoczesne dostęp do tego samego wystąpienia `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="10dc3-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="10dc3-153">Zapominanie do oczekiwania na ukończenie operacji asynchronicznej przed rozpoczęciem jakiejkolwiek innej operacji w tym samym kontekście DbContext</span><span class="sxs-lookup"><span data-stu-id="10dc3-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="10dc3-154">Metody asynchroniczne umożliwiają EF Core inicjowania operacji, które uzyskują dostęp do bazy danych w sposób nieblokowany.</span><span class="sxs-lookup"><span data-stu-id="10dc3-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="10dc3-155">Ale jeśli obiekt wywołujący nie oczekuje na zakończenie jednej z tych metod, a następnie wykonuje inne operacje na `DbContext`, stan `DbContext` może być (i bardzo prawdopodobnie będzie) uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="10dc3-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="10dc3-156">Zawsze Czekaj na EF Core metod asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="10dc3-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="10dc3-157">Niejawnie udostępnianie wystąpień DbContext w wielu wątkach przy użyciu iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="10dc3-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="10dc3-158">Metoda rozszerzenia [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) domyślnie rejestruje typy `DbContext` z [okresem istnienia w zakresie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) .</span><span class="sxs-lookup"><span data-stu-id="10dc3-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="10dc3-159">Jest to bezpieczne z równoczesnych problemów z dostępem w większości aplikacji ASP.NET Core, ponieważ w danym momencie istnieje tylko jeden wątek wykonujący każde żądanie klienta, a ponieważ każde żądanie pobiera oddzielny zakres iniekcji zależności (i w związku z tym oddzielnym wystąpieniem `DbContext`).</span><span class="sxs-lookup"><span data-stu-id="10dc3-159">This is safe from concurrent access issues in most ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span> <span data-ttu-id="10dc3-160">W przypadku modelu hostingu serwera Blazor jedno żądanie logiczne jest używane do obsługi obwodu użytkownika Blazor i w związku z tym w przypadku użycia domyślnego zakresu iniekcji dostępne jest tylko jedno wystąpienie DbContext o określonym zakresie.</span><span class="sxs-lookup"><span data-stu-id="10dc3-160">For Blazor Server hosting model, one logical request is used for maintaining the Blazor user circuit, and thus only one scoped DbContext instance is available per user circuit if the default injection scope is used.</span></span>

<span data-ttu-id="10dc3-161">Każdy kod, który jawnie wykonuje wiele wątków równolegle, powinien mieć pewność, że `DbContext` wystąpienia nie są kiedykolwiek dostępne jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="10dc3-161">Any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="10dc3-162">Przy użyciu iniekcji zależności można to osiągnąć przez zarejestrowanie kontekstu jako zakresu i Tworzenie zakresów (przy użyciu `IServiceScopeFactory`) dla każdego wątku lub przez zarejestrowanie `DbContext` jako przejściowego (przy użyciu przeciążenia `AddDbContext`, które przyjmuje parametr `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="10dc3-162">Using dependency injection, this can be achieved by either registering the context as scoped, and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="10dc3-163">Więcej informacji</span><span class="sxs-lookup"><span data-stu-id="10dc3-163">More reading</span></span>

- <span data-ttu-id="10dc3-164">Przeczytaj [wstrzyknięcie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , aby dowiedzieć się więcej o używaniu di.</span><span class="sxs-lookup"><span data-stu-id="10dc3-164">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="10dc3-165">Przeczytaj [test](testing/index.md) , aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="10dc3-165">Read [Testing](testing/index.md) for more information.</span></span>
