---
title: Konfigurowanie DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: ddabf825ef23c2ec07efcde390df7d0cf48db33c
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306509"
---
# <a name="configuring-a-dbcontext"></a>Konfigurowanie DbContext

W tym artykule przedstawiono podstawowe wzorce konfigurowania programu `DbContext` `DbContextOptions` za pośrednictwem programu w celu nawiązania połączenia z bazą danych przy użyciu określonego dostawcy EF Core i opcjonalnych zachowań.

## <a name="design-time-dbcontext-configuration"></a>Konfiguracja DbContext czasu projektowania

EF Core narzędzia czasu projektowania, takie jak [migracje](xref:core/managing-schemas/migrations/index) , muszą mieć możliwość odnajdywania i tworzenia działającego wystąpienia `DbContext` typu w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych. Ten proces może być automatyczny, tak długo, jak narzędzie można łatwo utworzyć `DbContext` w taki sposób, że zostanie ono skonfigurowane podobnie do konfiguracji w czasie wykonywania.

Chociaż każdy wzorzec, który zapewnia niezbędne informacje `DbContext` konfiguracyjne, może działać w czasie wykonywania, narzędzia, które wymagają `DbContext` użycia w czasie projektowania, mogą pracować tylko z ograniczoną liczbą wzorców. Są one omówione bardziej szczegółowo w sekcji [Tworzenie kontekstu w czasie projektowania](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Konfigurowanie DbContextOptions

`DbContext`musi mieć wystąpienie, `DbContextOptions` aby można było wykonać dowolną ilość pracy. `DbContextOptions` Wystąpienie przenosi informacje o konfiguracji, takie jak:

- Dostawca bazy danych do użycia, zazwyczaj wybierany przez wywołanie metody, takiej jak `UseSqlServer` lub `UseSqlite`. Te metody rozszerzające wymagają odpowiedniego pakietu dostawcy, takiego jak `Microsoft.EntityFrameworkCore.SqlServer` lub `Microsoft.EntityFrameworkCore.Sqlite`. Metody są zdefiniowane w `Microsoft.EntityFrameworkCore` przestrzeni nazw.
- Wszystkie niezbędne parametry połączenia lub identyfikator wystąpienia bazy danych, zwykle przesyłane jako argument do metody wyboru dostawcy wymienione powyżej
- Wszystkie selektory nieopcjonalnych zachowań na poziomie dostawcy, zwykle również łańcucha wewnątrz wywołania metody wyboru dostawcy
- Wszystkie selektory zachowań ogólnych EF Core, zwykle łańcucha po lub przed metodą selektora dostawcy

Poniższy przykład konfiguruje `DbContextOptions` używanie dostawcy SQL Server, połączenie zawarte `connectionString` w zmiennej, przekroczenie limitu czasu polecenia dostawcy i selektor zachowania EF Core, które wykonuje `DbContext` wszystkie zapytania wykonywane w Domyślnie [nie śledzone](xref:core/querying/tracking#no-tracking-queries) :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metody selektora dostawcy i inne metody selektora zachowań wymienione powyżej to metody `DbContextOptions` rozszerzające dotyczące klas opcji lub specyficznych dla dostawcy. Aby można było uzyskać dostęp do tych metod rozszerzających, może być konieczne posiadanie przestrzeni nazw ( `Microsoft.EntityFrameworkCore`zazwyczaj) w zakresie i uwzględnienie dodatkowych zależności pakietu w projekcie.

Można dostarczyć do obiektu, zastępując `OnConfiguring` metodę lub zewnętrznie za pośrednictwem argumentu konstruktora. `DbContext` `DbContextOptions`

Jeśli oba są używane, `OnConfiguring` są stosowane jako ostatnie i mogą zastąpić opcje dostarczone do argumentu konstruktora.

### <a name="constructor-argument"></a>Argument konstruktora

Kod kontekstu z konstruktorem:

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
> Konstruktor podstawowy DbContext również akceptuje nieogólną wersję `DbContextOptions`, ale użycie nieogólnej wersji nie jest zalecane w przypadku aplikacji z wieloma typami kontekstu.

Kod aplikacji do zainicjowania z argumentu konstruktora:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Trwa konfigurowanie

Kod kontekstu z `OnConfiguring`:

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

Kod aplikacji do zainicjowania `DbContext` , który `OnConfiguring`używa:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Takie podejście nie nadaje się do testowania, chyba że testy są przeznaczone dla całej bazy danych.

### <a name="using-dbcontext-with-dependency-injection"></a>Używanie DbContext z iniekcją zależności

EF Core obsługuje używanie `DbContext` z kontenerem iniekcji zależności. Typ kontekstu DbContext można dodać do kontenera usługi przy użyciu `AddDbContext<TContext>` metody.

`AddDbContext<TContext>`nastąpi zarówno typ kontekstu DB, `TContext`jak i odpowiednie `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.

Aby uzyskać dodatkowe informacje na temat iniekcji zależności, zobacz [więcej](#more-reading) informacji poniżej.

`DbContext` Dodawanie do iniekcji zależności:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Wymaga to dodania [argumentu konstruktora](#constructor-argument) do typu DbContext, który akceptuje `DbContextOptions<TContext>`.

Kod kontekstu:

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

Kod aplikacji (bezpośrednio, rzadziej używany):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Unikanie problemów wielowątkowości DbContext

Entity Framework Core nie obsługuje wielu operacji równoległych wykonywanych w tym samym `DbContext` wystąpieniu. Obejmuje to zarówno równoległe wykonywanie zapytań asynchronicznych, jak i jawne jednoczesne użycie z wielu wątków. W związku z `await` tym zawsze asynchroniczne wywołania są natychmiast lub `DbContext` używają osobnych wystąpień dla operacji wykonywanych równolegle.

Gdy EF Core wykryje próbę użycia `DbContext` wystąpienia współbieżnie, `InvalidOperationException` zobaczysz komunikat o takiej postaci: 

> Druga operacja rozpoczęta w tym kontekście przed ukończeniem poprzedniej operacji. Jest to zazwyczaj spowodowane przez różne wątki korzystające z tego samego wystąpienia DbContext, jednak elementy członkowskie wystąpienia nie są gwarantowane bezpieczny wątkowo.

Gdy dostęp współbieżny nie zostanie wykryty, może to spowodować niezdefiniowane zachowanie, awarie aplikacji i uszkodzenie danych.

Istnieją typowe błędy, które mogą przypadkowo spowodować jednoczesne dostęp do tego samego `DbContext` wystąpienia:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Zapominanie do oczekiwania na ukończenie operacji asynchronicznej przed rozpoczęciem jakiejkolwiek innej operacji w tym samym kontekście DbContext

Metody asynchroniczne umożliwiają EF Core inicjowania operacji, które uzyskują dostęp do bazy danych w sposób nieblokowany. Ale jeśli obiekt wywołujący nie oczekuje na zakończenie jednej z tych metod, a następnie wykonuje inne operacje na `DbContext`, stan `DbContext` może być (i bardzo prawdopodobnie będzie) uszkodzony. 

Zawsze Czekaj na EF Core metod asynchronicznych.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Niejawnie udostępnianie wystąpień DbContext w wielu wątkach przy użyciu iniekcji zależności

Metoda rozszerzająca domyślnie `DbContext` rejestruje typy z [okresem istnienia w zakresie.](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 

Jest to bezpieczne z równoczesnych problemów z dostępem w aplikacjach ASP.NET Core, ponieważ w danym momencie istnieje tylko jeden wątek wykonujący każde żądanie klienta, a ponieważ każde żądanie pobiera oddzielny zakres iniekcji zależności ( `DbContext` i w związku z tym oddzielny wystąpienie).

Jednak każdy kod, który jawnie wykonuje wiele wątków równolegle, powinien mieć `DbContext` pewność, że wystąpienia nie są kiedykolwiek dostępne współbieżnie.

Przy użyciu iniekcji zależności można to osiągnąć przez zarejestrowanie kontekstu jako zakresu i Tworzenie zakresów (za pomocą `IServiceScopeFactory`) dla każdego wątku lub przez `DbContext` zarejestrowanie jako przejściowego `AddDbContext` (przy użyciu przeciążenia, które przyjmuje `ServiceLifetime` parametr).

## <a name="more-reading"></a>Więcej informacji

* Przeczytaj [wprowadzenie na ASP.NET Core](../get-started/aspnetcore/index.md) , aby uzyskać więcej informacji na temat używania EF z ASP.NET Core.
* Przeczytaj [wstrzyknięcie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , aby dowiedzieć się więcej o używaniu di.
* Przeczytaj [test](testing/index.md) , aby uzyskać więcej informacji.
