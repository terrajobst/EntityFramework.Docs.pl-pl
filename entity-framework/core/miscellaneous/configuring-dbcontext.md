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
# <a name="configuring-a-dbcontext"></a>Konfigurowanie typu DbContext

W tym artykule przedstawiono podstawowe wzorce dotyczące konfigurowania `DbContext` za pośrednictwem `DbContextOptions` nawiązać połączenia z bazą danych, używając określonego dostawcy programu EF Core i opcjonalnie zachowania.

## <a name="design-time-dbcontext-configuration"></a>Konfiguracja typu DbContext w czasie projektowania

EF Core z czasu projektowania narzędzi, takich jak [migracje](xref:core/managing-schemas/migrations/index) muszą mieć możliwość odnajdywania i utworzyć wystąpienie pracy `DbContext` typu, aby można było zbierać szczegółowe informacje dotyczące typów jednostek i sposobu mapowania ich na schemat bazy danych aplikacji. Ten proces może być automatyczna, tak długo, jak łatwo można utworzyć narzędzie `DbContext` w taki sposób, że zostanie on skonfigurowany podobnie jak może zostać skonfigurowane w czasie wykonywania.

Podczas gdy wszelkie wzorzec, który dostarcza informacje o konfiguracji niezbędne do `DbContext` może pracować w czasie wykonywania, narzędzi, które wymagają przy użyciu `DbContext` w czasie projektowania może pracować tylko z ograniczonej liczby wzorców. Są one objęte bardziej szczegółowo w [czasu projektowania, tworzenia kontekstu](xref:core/miscellaneous/cli/dbcontext-creation) sekcji.

## <a name="configuring-dbcontextoptions"></a>Konfigurowanie DbContextOptions

`DbContext` musi mieć instancję `DbContextOptions` aby można było wykonać wszelkie prace. `DbContextOptions` Wystąpienia niesie ze sobą informacje o konfiguracji takich jak:

- Dostawca bazy danych do użycia, zwykle wybrane przez wywołanie metody, takie jak `UseSqlServer` lub `UseSqlite`
- Wszelkie wymagane parametry lub identyfikator wystąpienia bazy danych zazwyczaj przekazywany jako argument do podanej powyżej metody wyboru dostawcy
- Żadnych selektorów poziom dostawcy, zachowanie opcjonalne, również są zazwyczaj powiązane wewnątrz wywołania metody wyboru dostawcy
- Wszelkie ogólne selektorów zachowanie programu EF Core, zwykle połączonymi w łańcuch po lub przed metodą selektor dostawcy

Poniższy przykład umożliwia skonfigurowanie `DbContextOptions` do używania dostawcy programu SQL Server, połączenie jest zawarty w `connectionString` zmienną, limit czasu polecenia poziom dostawcy i selektor zachowanie programu EF Core, która sprawia, że wszystkie zapytania wykonywane w `DbContext` [śledzenia nie](xref:core/querying/tracking#no-tracking-queries) domyślnie:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Dostawca selektora metody i innych metod selektor zachowanie wymienione powyżej są metody rozszerzenia na `DbContextOptions` lub klasy opcji właściwe dla dostawcy. Aby uzyskać dostęp do tych metod rozszerzenia może być konieczne przestrzeń nazw (zazwyczaj `Microsoft.EntityFrameworkCore`) w zakres i obejmują zależności dodatkowych pakietów w projekcie.

`DbContextOptions` Mogą być dostarczane do `DbContext` przez zastąpienie `OnConfiguring` metody lub zewnętrznie za pośrednictwem argumentu konstruktora.

Jeśli używane są obie, `OnConfiguring` ostatnio zastosowane i mogą zastąpić opcje przekazana do argumentu konstruktora.

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
> Podstawowy Konstruktor typu DbContext akceptuje także nieogólnego wersję `DbContextOptions`, ale przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji z wieloma typami kontekstu.

Kod aplikacji można zainicjować na podstawie argumentu konstruktora:

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

Kod aplikacji, aby zainicjować `DbContext` , który używa `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> To podejście nie jest przystosowany do testowania, chyba że próby docelowe pełnej bazy danych.

### <a name="using-dbcontext-with-dependency-injection"></a>Przy użyciu iniekcji zależności typu DbContext

EF Core obsługuje korzystanie z `DbContext` z kontenera iniekcji zależności. Typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>` metody.

`AddDbContext<TContext>` spowoduje, że oba danego typu DbContext `TContext`i odpowiedni `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.

Zobacz [więcej odczytu](#more-reading) poniżej dodatkowe informacje na temat iniekcji zależności.

Dodawanie `Dbcontext` do wstrzykiwania zależności:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

To wymaga dodania [argumentu konstruktora](#constructor-argument) do danego typu DbContext, który akceptuje `DbContextOptions<TContext>`.

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

Kod aplikacji (w programie ASP.NET Core):

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

## <a name="more-reading"></a>Odczytywanie więcej

* Odczyt [wprowadzenie do programu ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji na temat korzystania z programów EF z platformą ASP.NET Core.
* Odczyt [wstrzykiwanie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Aby dowiedzieć się więcej o korzystaniu z DI.
* Odczyt [testowania](testing/index.md) Aby uzyskać więcej informacji.
