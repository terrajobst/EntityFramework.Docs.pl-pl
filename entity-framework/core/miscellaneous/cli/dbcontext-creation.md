---
title: Tworzenie DbContext w czasie projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197575"
---
<a name="design-time-dbcontext-creation"></a>Tworzenie klasy DbContext w czasie projektowania
==============================
Niektóre polecenia narzędzi EF Core (na przykład polecenia [migracji][1] ) wymagają utworzenia wystąpienia pochodnego `DbContext` w czasie projektowania w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych. W większości przypadków wskazane `DbContext` jest, aby utworzone w ten sposób jest skonfigurowane w podobny sposób jak w [czasie wykonywania][2].

Istnieją różne sposoby tworzenia `DbContext`:

<a name="from-application-services"></a>Z usług aplikacji
-------------------------
Jeśli projekt startowy używa hosta [sieci Web ASP.NET Core][3] lub [hosta ogólnego platformy .NET Core][4], narzędzia próbują uzyskać obiekt DbContext od dostawcy usług aplikacji.

Narzędzia najpierw próbują uzyskać dostawcę `Program.CreateHostBuilder()`usług, wywołując wywoływanie `Build()`, a następnie uzyskując dostęp `Services` do właściwości.

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

`DbContext` Sama i wszystkie zależności w jego konstruktorze muszą być zarejestrowane jako usługi w dostawcy usług aplikacji. Można to łatwo osiągnąć poprzez posiadanie [ `DbContext` konstruktora na, który przyjmuje wystąpienie `DbContextOptions<TContext>` ][5] [ `AddDbContext<TContext>` ][6]jako argument i przy użyciu metody.

<a name="using-a-constructor-with-no-parameters"></a>Korzystanie z konstruktora bez parametrów
--------------------------------------
Jeśli nie można uzyskać elementu DbContext od dostawcy usług aplikacji, narzędzia szukają typu pochodnego `DbContext` wewnątrz projektu. Następnie próbują utworzyć wystąpienie przy użyciu konstruktora bez parametrów. Może to być Konstruktor domyślny, jeśli `DbContext` jest skonfigurowany [`OnConfiguring`][7] przy użyciu metody.

<a name="from-a-design-time-factory"></a>Z fabryki czasu projektowania
--------------------------
Możesz również poinformować o narzędziach, jak utworzyć DbContext, implementując `IDesignTimeDbContextFactory<TContext>` interfejs: Jeśli klasa implementująca ten interfejs znajduje się w tym samym projekcie co pochodna `DbContext` lub w projekcie startowym aplikacji, narzędzia te pomijają inne sposoby tworzenia kontekstu DBI używają zamiast nich fabryki czasu projektowania.

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
> `args` Parametr jest aktualnie nieużywany. Występuje [problem][8] ze śledzeniem możliwości określania argumentów czasu projektowania z narzędzi.

Fabryka czasu projektowania może być szczególnie przydatna, jeśli trzeba skonfigurować DbContext w inny sposób niż czas projektowania niż w czasie wykonywania, jeśli `DbContext` Konstruktor przyjmuje dodatkowe parametry nie są zarejestrowane w programie di, jeśli nie są używane w ogóle, lub jeśli w przypadku niektórych powód, dla którego wolisz, `BuildWebHost` aby nie mieć metody w `Main` klasie ASP.NET Core aplikacji.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
