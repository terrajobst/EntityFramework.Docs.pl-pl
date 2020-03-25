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
# <a name="configuring-a-dbcontext"></a>Konfigurowanie klasy DbContext

W tym artykule przedstawiono podstawowe wzorce konfigurowania `DbContext` za pośrednictwem `DbContextOptions` do łączenia się z bazą danych przy użyciu określonego dostawcy EF Core i opcjonalnych zachowań.

## <a name="design-time-dbcontext-configuration"></a>Konfiguracja DbContext czasu projektowania

EF Core narzędzia czasu projektowania, takie jak [migracje](xref:core/managing-schemas/migrations/index) , muszą mieć możliwość odnajdywania i tworzenia działającego wystąpienia typu `DbContext` w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych. Ten proces może być automatyczny, o ile narzędzie może łatwo utworzyć `DbContext` w taki sposób, że zostanie on skonfigurowany podobnie do konfiguracji w czasie wykonywania.

Chociaż każdy wzorzec zapewniający niezbędne informacje konfiguracyjne dla `DbContext` może działać w czasie wykonywania, narzędzia, które wymagają użycia `DbContext` w czasie projektowania, mogą pracować tylko z ograniczoną liczbą wzorców. Są one omówione bardziej szczegółowo w sekcji [Tworzenie kontekstu w czasie projektowania](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Konfigurowanie DbContextOptions

`DbContext` musi mieć wystąpienie `DbContextOptions`, aby można było wykonać dowolną ilość pracy. Wystąpienie `DbContextOptions` przenosi informacje o konfiguracji, takie jak:

- Dostawca bazy danych do użycia, zazwyczaj wybierany przez wywołanie metody, takiej jak `UseSqlServer` lub `UseSqlite`. Te metody rozszerzające wymagają odpowiedniego pakietu dostawcy, takiego jak `Microsoft.EntityFrameworkCore.SqlServer` lub `Microsoft.EntityFrameworkCore.Sqlite`. Metody są zdefiniowane w przestrzeni nazw `Microsoft.EntityFrameworkCore`.
- Wszystkie niezbędne parametry połączenia lub identyfikator wystąpienia bazy danych, zwykle przesyłane jako argument do metody wyboru dostawcy wymienione powyżej
- Wszystkie selektory nieopcjonalnych zachowań na poziomie dostawcy, zwykle również łańcucha wewnątrz wywołania metody wyboru dostawcy
- Wszystkie selektory zachowań ogólnych EF Core, zwykle łańcucha po lub przed metodą selektora dostawcy

Poniższy przykład konfiguruje `DbContextOptions` do korzystania z dostawcy SQL Server, połączenie zawarte w zmiennej `connectionString`, przekroczenie limitu czasu polecenia dostawcy i selektor zachowania EF Core, które domyślnie wykonuje wszystkie zapytania wykonywane w `DbContext` [bez śledzenia](xref:core/querying/tracking#no-tracking-queries) :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metody selektora dostawcy i inne metody selektora zachowań wymienione powyżej to metody rozszerzające dotyczące `DbContextOptions` lub klas opcji specyficznych dla dostawcy. Aby mieć dostęp do tych metod rozszerzających, może być konieczne posiadanie przestrzeni nazw (zwykle `Microsoft.EntityFrameworkCore`) w zakresie i uwzględnienie w projekcie dodatkowych zależności pakietów.

`DbContextOptions` można dostarczyć do `DbContext` przez zastąpienie metody `OnConfiguring` lub zewnętrznie za pośrednictwem argumentu konstruktora.

Jeśli oba są używane, `OnConfiguring` jest stosowany jako ostatni i mogą zastąpić opcje dostarczone do argumentu konstruktora.

### <a name="constructor-argument"></a>Argument konstruktora

Konstruktor może po prostu zaakceptować `DbContextOptions` w następujący sposób:

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

Aplikacja może teraz przekazać `DbContextOptions` podczas tworzenia wystąpienia kontekstu w następujący sposób:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Trwa konfigurowanie

Możesz również zainicjować `DbContextOptions` w samym kontekście. Chociaż ta technika jest używana w konfiguracji podstawowej, zwykle nadal trzeba uzyskać pewne szczegóły konfiguracji z zewnątrz, np. parametry połączenia z bazą danych. Można to zrobić za pomocą interfejsu API konfiguracji lub w inny sposób.

Aby zainicjować `DbContextOptions` w kontekście, Zastąp metodę `OnConfiguring` i wywołaj metody z podanej `DbContextOptionsBuilder`:

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

Aplikacja może po prostu utworzyć wystąpienie takiego kontekstu bez przekazywania wszystkiego do jego konstruktora:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Takie podejście nie nadaje się do testowania, chyba że testy są przeznaczone dla całej bazy danych.

### <a name="using-dbcontext-with-dependency-injection"></a>Używanie DbContext z iniekcją zależności

EF Core obsługuje używanie `DbContext` z kontenerem iniekcji zależności. Typ kontekstu DbContext można dodać do kontenera usługi za pomocą metody `AddDbContext<TContext>`.

`AddDbContext<TContext>` spowoduje, że zarówno typ kontekstu DB, `TContext`, jak i odpowiednie `DbContextOptions<TContext>` dostępne do iniekcji z kontenera usług.

Aby uzyskać dodatkowe informacje na temat iniekcji zależności, zobacz [więcej](#more-reading) informacji poniżej.

Dodawanie `DbContext` do iniekcji zależności:

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

Entity Framework Core nie obsługuje wielu operacji równoległych dla tego samego wystąpienia `DbContext`. Obejmuje to zarówno równoległe wykonywanie zapytań asynchronicznych, jak i jawne jednoczesne użycie z wielu wątków. W związku z tym zawsze należy natychmiast `await` asynchroniczne wywołania lub używać oddzielnych wystąpień `DbContext` dla operacji wykonywanych równolegle.

Gdy EF Core wykryje próbę użycia wystąpienia `DbContext` współbieżnie, zobaczysz `InvalidOperationException` z komunikatem podobnym do tego:

> Druga operacja rozpoczęta w tym kontekście przed ukończeniem poprzedniej operacji. Jest to zazwyczaj spowodowane przez różne wątki korzystające z tego samego wystąpienia DbContext, jednak elementy członkowskie wystąpienia nie są gwarantowane bezpieczny wątkowo.

Gdy dostęp współbieżny nie zostanie wykryty, może to spowodować niezdefiniowane zachowanie, awarie aplikacji i uszkodzenie danych.

Istnieją typowe błędy, które mogą przypadkowo spowodować jednoczesne dostęp do tego samego wystąpienia `DbContext`:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Zapominanie do oczekiwania na ukończenie operacji asynchronicznej przed rozpoczęciem jakiejkolwiek innej operacji w tym samym kontekście DbContext

Metody asynchroniczne umożliwiają EF Core inicjowania operacji, które uzyskują dostęp do bazy danych w sposób nieblokowany. Ale jeśli obiekt wywołujący nie oczekuje na zakończenie jednej z tych metod, a następnie wykonuje inne operacje na `DbContext`, stan `DbContext` może być (i bardzo prawdopodobnie będzie) uszkodzony.

Zawsze Czekaj na EF Core metod asynchronicznych.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Niejawnie udostępnianie wystąpień DbContext w wielu wątkach przy użyciu iniekcji zależności

Metoda rozszerzenia [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) domyślnie rejestruje typy `DbContext` z [okresem istnienia w zakresie](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) .

Jest to bezpieczne z równoczesnych problemów z dostępem w większości aplikacji ASP.NET Core, ponieważ w danym momencie istnieje tylko jeden wątek wykonujący każde żądanie klienta, a ponieważ każde żądanie pobiera oddzielny zakres iniekcji zależności (i w związku z tym oddzielnym wystąpieniem `DbContext`). W przypadku modelu hostingu serwera Blazor jedno żądanie logiczne jest używane do obsługi obwodu użytkownika Blazor i w związku z tym w przypadku użycia domyślnego zakresu iniekcji dostępne jest tylko jedno wystąpienie DbContext o określonym zakresie.

Każdy kod, który jawnie wykonuje wiele wątków równolegle, powinien mieć pewność, że `DbContext` wystąpienia nie są kiedykolwiek dostępne jednocześnie.

Przy użyciu iniekcji zależności można to osiągnąć przez zarejestrowanie kontekstu jako zakresu i Tworzenie zakresów (przy użyciu `IServiceScopeFactory`) dla każdego wątku lub przez zarejestrowanie `DbContext` jako przejściowego (przy użyciu przeciążenia `AddDbContext`, które przyjmuje parametr `ServiceLifetime`).

## <a name="more-reading"></a>Więcej informacji

- Przeczytaj [wstrzyknięcie zależności](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) , aby dowiedzieć się więcej o używaniu di.
- Przeczytaj [test](testing/index.md) , aby uzyskać więcej informacji.
