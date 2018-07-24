---
title: Tworzenie typu DbContext w czasie projektowania - programu EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 648ca990252fb32d8cf181a7ae672d07a81f56bb
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201922"
---
<a name="design-time-dbcontext-creation"></a>Tworzenie typu DbContext w czasie projektowania
==============================
Niektóre polecenia EF Core Tools (na przykład [migracje] [ 1] poleceń) wymagają pochodnej `DbContext` wystąpienia, które ma zostać utworzony w czasie projektowania, aby można było zbierać szczegółowe informacje o aplikacji typy jednostek i sposobu mapowania ich na schemat bazy danych. W większości przypadków jest pożądane, `DbContext` utworzone w ten sposób jest skonfigurowany w podobny sposób, jak będzie [skonfigurowane w czasie wykonywania][2].

Istnieją różne sposoby, narzędzia, spróbuj utworzyć `DbContext`:

<a name="from-application-services"></a>Z usługi aplikacji
-------------------------
Jeśli Twój projekt startowy aplikacji ASP.NET Core, narzędzia próby uzyskania obiektu DbContext od dostawcy usług w aplikacji.

Narzędzie najpierw spróbują uzyskać dostawcę usługi za pomocą wywołania `Program.BuildWebHost()` i uzyskiwania dostępu do `IWebHost.Services` właściwości.

> [!NOTE]
> Podczas tworzenia nowej aplikacji platformy ASP.NET Core 2.0 tego punktu zaczepienia jest domyślnie. W poprzednich wersjach programu EF Core i ASP.NET Core, narzędzia próbuj wywoływać `Startup.ConfigureServices` bezpośrednio w celu uzyskania dostawcy usług aplikacji, ale ten wzorzec, przestanie działać poprawnie w aplikacjach ASP.NET Core 2.0. Jeśli uaktualniasz aplikacji ASP.NET Core 1.x do 2.0, możesz to zrobić [zmodyfikować swoje `Program` klasy oparte na wzorcu nowe][3].

`DbContext` Sam, jak i wszelkich zależności w jego konstruktorze muszą być zarejestrowani jako usługi w aplikacji usługodawcy. Można to łatwo osiągnąć dzięki [Konstruktor w `DbContext` przyjmującej wystąpienie `DbContextOptions<TContext>` jako argument] [ 4] i przy użyciu [ `AddDbContext<TContext>` —Metoda] [5].

<a name="using-a-constructor-with-no-parameters"></a>Za pomocą konstruktora bez parametrów
--------------------------------------
Jeśli kontekst DbContext nie można uzyskać od dostawcy usług w aplikacji, wyszukaj narzędzia pochodnej `DbContext` typów wewnątrz projektu. Następnie próby utworzenia wystąpienia przy użyciu konstruktora bez parametrów. Może to być domyślnego konstruktora, jeśli `DbContext` została skonfigurowana przy użyciu [ `OnConfiguring` ] [ 6] metody.

<a name="from-a-design-time-factory"></a>Z fabryki czasu projektowania
--------------------------
Możesz również poinformować narzędzia sposób tworzenia usługi DbContext implementując `IDesignTimeDbContextFactory<TContext>` interfejsu: Jeśli klasy implementującej interfejs ten jest odnaleziony ani w tym samym projekcie jako pochodnej `DbContext` lub w projekcie uruchamiania aplikacji, obejście narzędzia inne sposoby tworzenia DbContext i używania fabryki czasu projektowania, a zamiast tego.

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
> `args` Parametru jest aktualnie używana. Brak [problemu] [ 7] śledzenia możliwość określenia czasu projektowania argumentów z poziomu narzędzi.

Fabryka projektowania może być szczególnie przydatne, jeśli musisz skonfigurować kontekstu DbContext inaczej czas projektowania niż w czasie wykonywania, jeśli `DbContext` Konstruktor przyjmuje dodatkowych parametrów nie są zarejestrowane w DI, jeśli nie używasz DI wcale, lub w przypadku niektórych przyczyny nie chcesz mieć `BuildWebHost` metody w aplikacji platformy ASP.NET Core `Main` klasy.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
