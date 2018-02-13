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
---
# <a name="configuring-a-dbcontext"></a>Konfigurowanie obiektu DbContext

W tym artykule przedstawiono wzorców podstawowych konfigurowania `DbContext` za pośrednictwem `DbContextOptions` nawiązać połączenia z bazą danych przy użyciu określonego dostawcy EF Core i opcjonalnie zachowania.

## <a name="design-time-dbcontext-configuration"></a>Konfiguracja obiektu DbContext czasu projektowania

Czasu projektowania EF podstawowych narzędzi, takich jak [migracje](xref:core/managing-schemas/migrations/index) muszą mieć możliwość odnajdywania i Utwórz wystąpienie pracy `DbContext` typu w celu zbierania szczegóły dotyczące typów jednostek aplikacji oraz sposobu mapowania ich na schemat bazy danych. Ten proces może być automatyczna, tak długo, jak łatwo utworzyć narzędzie `DbContext` w taki sposób, że będzie on podobnie skonfigurowany sposób może zostać skonfigurowana w czasie wykonywania.

Podczas żadnych wzorzec, który zawiera informacje o konfiguracji niezbędne do `DbContext` może działać w czasie wykonywania, narzędzi, które wymagają przy użyciu `DbContext` w czasie projektowania może pracować tylko z ograniczoną liczbą wzorców. Te są opisane bardziej szczegółowo w [tworzenie kontekstu w czasie projektowania](xref:core/miscellaneous/cli/dbcontext-creation) sekcji.

## <a name="configuring-dbcontextoptions"></a>Konfigurowanie DbContextOptions

`DbContext` musi być wystąpieniem `DbContextOptions` w celu wykonywania pracy. `DbContextOptions` Wystąpienia niesie informacje o konfiguracji, takich jak:

- Dostawca bazy danych do użycia, zazwyczaj wybierana przez wywołanie metody, takie jak `UseSqlServer` lub `UseSqlite`
- Wszystkie niezbędne parametry lub identyfikator wystąpienia bazy danych zazwyczaj przekazanego jako argument do metody wyboru dostawcy wymienione powyżej
- Wszystkie selektory zachowanie opcjonalne poziomie dostawcy, zwykle także powiązane wewnątrz wywołania metody wyboru dostawcy
- Wszelkie ogólne selektorów zachowanie EF Core, zwykle łańcuchowej po lub przed wywołaniem metody selektora dostawcy

Poniższy przykład konfiguruje `DbContextOptions` korzysta z dostawcy programu SQL Server, połączenie zawarte w `connectionString` zmiennej, limit czasu polecenia poziomie dostawcy i selektor zachowanie EF Core sprawia, że wszystkie zapytania wykonywane w `DbContext` [śledzenia nie](xref:core/querying/tracking#no-tracking-queries) domyślnie:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metody selektora dostawcy i innych metod selektora zachowanie wymienione powyżej są metody rozszerzenia na `DbContextOptions` lub klasy opcji specyficznych dla dostawcy. Aby uzyskać dostęp do tych metod rozszerzenia może być konieczne przestrzeń nazw (zazwyczaj `Microsoft.EntityFrameworkCore`) w zakres i zawierać pakiet dodatkowe zależności w projekcie.

`DbContextOptions` Mogą być dostarczane do `DbContext` przez zastąpienie `OnConfiguring` metody lub zewnętrznie za pośrednictwem argumentów konstruktora.

Jeśli używana zarówno `OnConfiguring` ostatnio zastosowane i mogą zastąpić opcje przekazany do argumentu konstruktora.

### <a name="constructor-argument"></a>Argument konstruktora

Kontekst kodu za pomocą konstruktora:

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
> Podstawowy Konstruktor DbContext również zaakceptowanie wersji nieogólnego `DbContextOptions`, ale przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji o wiele typów kontekstu.

Kod aplikacji, można zainicjować na podstawie argumentów konstruktora:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Kontekst kodu za pomocą `OnConfiguring`:

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

Kod aplikacji w celu zainicjowania `DbContext` używającą `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Takie podejście nie nadają się do testowania, chyba, że testy target pełnej bazy danych.

### <a name="using-dbcontext-with-dependency-injection"></a>Przy użyciu obiektu DbContext z iniekcji zależności

EF Core obsługuje korzystanie z `DbContext` z kontenera iniekcji zależności. Danego typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>` metody.

`AddDbContext<TContext>` spowoduje, że oba danego typu DbContext `TContext`i odpowiadający mu `DbContextOptions<TContext>` dostępne iniekcji z kontenera usług.

Zobacz [więcej odczytu](#more-reading) poniżej dodatkowe informacje na temat iniekcji zależności.

Dodawanie `Dbcontext` do iniekcji zależności:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Wymaga to dodawanie [argument konstruktora](#constructor-argument) Twojego typ DbContext, który akceptuje `DbContextOptions<TContext>`.

Kontekst kodu:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Kod aplikacji (w ASP.NET Core):

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

Kod aplikacji (przy użyciu element ServiceProvider bezpośrednio, mniej typowe):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Więcej odczytu

* Odczyt [wprowadzenie na platformy ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji o używaniu EF z platformy ASP.NET Core.
* Odczyt [iniekcji zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Aby dowiedzieć się więcej o korzystaniu z Podpisane.
* Odczyt [testowanie](testing/index.md) Aby uzyskać więcej informacji.
