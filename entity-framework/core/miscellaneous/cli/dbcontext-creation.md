---
title: Tworzenie DbContext czasu projektowania - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/28/2018
---
<a name="design-time-dbcontext-creation"></a>Tworzenie typu DbContext w czasie projektowania
==============================
Niektóre polecenia EF podstawowe narzędzia (na przykład [migracje] [ 1] poleceń) wymagają pochodnego `DbContext` wystąpienia należy utworzyć w czasie projektowania w celu zbierania informacji o aplikacji typy jednostek oraz sposobu mapowania ich na schemat bazy danych. W większości przypadków jest pożądane który `DbContext` utworzone w ten sposób jest skonfigurowany w sposób podobny do sposobu byłoby [skonfigurowane w czasie wykonywania][2].

Istnieją różne sposoby, spróbuj utworzyć narzędzia `DbContext`:

<a name="from-application-services"></a>Z usługi aplikacji
-------------------------
Twój projekt startowy w przypadku aplikacji platformy ASP.NET Core, narzędzia próby uzyskania obiektu DbContext z dostawcy usług aplikacji.

Narzędzie najpierw spróbuj uzyskać dostawcę usługi za pomocą `Program.BuildWebHost()` i uzyskiwania dostępu do `IWebHost.Services` właściwości.

> [!NOTE]
> Podczas tworzenia nowej aplikacji ASP.NET Core 2.0 domyślnie znajduje się ta punktu zaczepienia. W poprzednich wersjach programu EF Core i ASP.NET Core, narzędzia próbuj wywoływać `Startup.ConfigureServices` bezpośrednio w celu uzyskania dostawcy usług aplikacji, ale ten wzorzec już nie działa prawidłowo w aplikacjach ASP.NET Core 2.0. Uaktualniania aplikacji platformy ASP.NET Core 1.x 2.0, można [zmodyfikować Twoje `Program` klasy do nowego wzorcem][3].

`DbContext` Się i zależności w swoich konstruktorach należy zarejestrować jako usługi w aplikacji usługodawcy. Można to łatwo osiągnąć dzięki użyciu [Konstruktor na `DbContext` pobierającej wystąpienia `DbContextOptions<TContext>` jako argument] [ 4] i przy użyciu [ `AddDbContext<TContext>` —Metoda] [5].

<a name="using-a-constructor-with-no-parameters"></a>Za pomocą konstruktora bez parametrów
--------------------------------------
Jeśli DbContext nie można uzyskać od dostawcy usług aplikacji, wyszukaj narzędzia pochodnej `DbContext` typ wewnątrz projektu. Następnie próby utworzenia wystąpienia przy użyciu konstruktora bez parametrów. Może to być domyślnego konstruktora, jeśli `DbContext` jest konfigurowana przy użyciu [ `OnConfiguring` ] [ 6] metody.

<a name="from-a-design-time-factory"></a>Z fabryki czasu projektowania
--------------------------
Możesz także wybrać narzędzia sposobu tworzenia sieci DbContext zaimplementowanie `IDesignTimeDbContextFactory<TContext>` interfejsu: Jeśli klasy implementującej interfejs ten znajduje się w jednym tego samego projektu jako pochodnej `DbContext` lub w aplikacji projekt startowy, obejście narzędzia inne metody tworzenia zamiast tego obiektu DbContext i używania fabryki czasu projektowania.

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
> `args` Parametr jest aktualnie używana. Brak [problemu] [ 7] śledzenia umożliwia określenie argumentów czasu projektowania w menu Narzędzia.

Fabryka czasu projektowania może być szczególnie przydatne, jeśli konieczne będzie skonfigurowanie kontekstu DbContext inaczej dla czasu projektowania niż w czasie wykonywania, jeśli `DbContext` Konstruktor ma dodatkowe parametry nie są zarejestrowane w Podpisane, jeśli nie używasz Podpisane na wszystkich lub, jeśli dla niektórych przyczyny nie chcesz mieć `BuildWebHost` metody w aplikacji platformy ASP.NET Core  
`Main` Klasa.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
