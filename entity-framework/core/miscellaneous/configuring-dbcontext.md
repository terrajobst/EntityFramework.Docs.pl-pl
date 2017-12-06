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
# <a name="configuring-a-dbcontext"></a>Konfigurowanie obiektu DbContext

W tym artykule przedstawiono wzorce konfigurowania `DbContext` z `DbContextOptions`. Opcje są używane głównie aby wybrać i skonfigurować Magazyn danych.

## <a name="configuring-dbcontextoptions"></a>Konfigurowanie DbContextOptions

`DbContext`musi być wystąpieniem `DbContextOptions` w celu wykonania. Tę funkcję można skonfigurować przez zastąpienie `OnConfiguring`, lub zewnętrznie dostarczony przez argument konstruktora.

Jeśli są używane, `OnConfiguring` jest wykonywana na podane opcje, co oznacza jest dodatek i może zastąpić opcje przekazany do argumentu konstruktora.

### <a name="constructor-argument"></a>Argument konstruktora

Kontekst kodu za pomocą konstruktora

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
> Podstawowy Konstruktor DbContext również zaakceptowanie wersji nieogólnego `DbContextOptions`. Przy użyciu wersji nieogólnego nie jest zalecane w przypadku aplikacji o wiele typów kontekstu.

Kod aplikacji, można zainicjować na podstawie argumentów konstruktora

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`Ostatnia i mogą zastąpić opcje uzyskane z Podpisane lub konstruktora. Takie podejście nie nadają się do testowania (chyba że zostanie rozpoczęta dla pełnej bazy danych).

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

Kod aplikacji, aby zainicjować z `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>Przy użyciu obiektu DbContext z iniekcji zależności

Obsługa EF przy użyciu `DbContext` z kontenera iniekcji zależności. Danego typu DbContext można dodać do kontenera usługi za pomocą `AddDbContext<TContext>`.

`AddDbContext`spowoduje, że oba danego typu DbContext `TContext`, i `DbContextOptions<TContext>` dostępne iniekcji z kontenera usług.

Zobacz [więcej odczytu](#more-reading) poniżej informacje na iniekcji zależności.

Dodawanie dbcontext do iniekcji zależności

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Wymaga to dodawanie [argument konstruktora](#constructor-argument) Twojego typ DbContext, który akceptuje `DbContextOptions`.

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
public MyController(BloggingContext context)
```

Kod aplikacji (przy użyciu element ServiceProvider bezpośrednio, mniej typowe):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Za pomocą`IDesignTimeDbContextFactory<TContext>`

Alternatywą dla powyższych opcji może również zapewniać implementację elementu `IDesignTimeDbContextFactory<TContext>`. Narzędzia EF umożliwia utworzenie wystąpienia DbContext z tej fabryki. Może to być wymagane w celu umożliwienia określonego środowiska czasu projektowania, takich jak migracji.

Implementuje ten interfejs, aby włączyć usługi czasu projektowania dla typów kontekstu, które nie ma publicznego konstruktora domyślnego. Usługi w czasie projektowania automatycznie wykryje implementacje tego interfejsu, które znajdują się w tym samym zestawie co kontekst pochodnych.

Przykład:

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

## <a name="more-reading"></a>Więcej odczytu

* Odczyt [wprowadzenie na platformy ASP.NET Core](../get-started/aspnetcore/index.md) Aby uzyskać więcej informacji o używaniu EF z platformy ASP.NET Core.
* Odczyt [iniekcji zależności](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) Aby dowiedzieć się więcej o korzystaniu z Podpisane.
* Odczyt [testowanie](testing/index.md) Aby uzyskać więcej informacji.
