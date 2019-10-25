---
title: Tworzenie DbContext w czasie projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812018"
---
# <a name="design-time-dbcontext-creation"></a>Tworzenie klasy DbContext w czasie projektowania

Niektóre polecenia narzędzi EF Core (na przykład polecenia [migracji][1] ) wymagają utworzenia wystąpienia pochodnego `DbContext`, które ma zostać utworzone w czasie projektowania w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych. W większości przypadków wskazane jest, aby `DbContext` utworzone w ten sposób został skonfigurowany w podobny sposób, jak zostanie [skonfigurowany w czasie wykonywania][2].

Istnieją różne sposoby tworzenia `DbContext`przez narzędzia:

## <a name="from-application-services"></a>Z usług aplikacji

Jeśli projekt startowy używa hosta [sieci Web ASP.NET Core][3] lub [hosta ogólnego platformy .NET Core][4], narzędzia próbują uzyskać obiekt DbContext od dostawcy usług aplikacji.

Narzędzia najpierw próbują uzyskać dostawcę usługi przez wywoływanie `Program.CreateHostBuilder()`, wywoływanie `Build()`, a następnie uzyskanie dostępu do właściwości `Services`.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Podczas tworzenia nowej aplikacji ASP.NET Core ten element Hook jest uwzględniany domyślnie.

Sama `DbContext` i wszelkie zależności w jego konstruktorze muszą być zarejestrowane jako usługi w dostawcy usług aplikacji. Można to łatwo osiągnąć poprzez posiadanie [konstruktora na `DbContext`, który przyjmuje wystąpienie `DbContextOptions<TContext>` jako argument][5] i przy użyciu [metody`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Korzystanie z konstruktora bez parametrów

Jeśli nie można uzyskać kontekstu DbContext od dostawcy usług aplikacji, narzędzia szukają typu pochodnego `DbContext` wewnątrz projektu. Następnie próbują utworzyć wystąpienie przy użyciu konstruktora bez parametrów. Może to być Konstruktor domyślny, jeśli `DbContext` jest skonfigurowany przy użyciu metody [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>Z fabryki czasu projektowania

Możesz również poinformować narzędzia, jak utworzyć DbContext, implementując interfejs `IDesignTimeDbContextFactory<TContext>`: Jeśli klasa implementująca ten interfejs znajduje się w tym samym projekcie, co pochodna `DbContext` lub w projekcie startowym aplikacji, narzędzia pomijają pozostałe sposoby tworzenia kontekstu DBI używania fabryki czasu projektowania.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> Parametr `args` nie jest obecnie używany. Występuje [problem][8] ze śledzeniem możliwości określania argumentów czasu projektowania z narzędzi.

Fabryka czasu projektowania może być szczególnie przydatna, jeśli trzeba skonfigurować DbContext dla czasu projektowania niż w czasie wykonywania, jeśli Konstruktor `DbContext` przyjmuje dodatkowe parametry nie są zarejestrowane w programie DI, jeśli nie używasz DI na pewno lub z jakiegoś powodu wolisz, aby nie mieć metody `BuildWebHost` w klasie `Main` aplikacji ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
