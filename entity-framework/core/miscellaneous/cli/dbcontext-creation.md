---
title: Tworzenie DbContext czasu projektowania - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Tworzenie typu DbContext w czasie projektowania
==============================
Niektóre narzędzia EF polecenia wymagają wystąpienie typu DbContext ma zostać utworzony na projekt czasu (na przykład podczas uruchamiania polecenia migracji). Istnieją różne sposoby narzędzia spróbuj utworzyć ją.

<a name="from-application-services"></a>Z usługi aplikacji
-------------------------
Twój projekt startowy w przypadku aplikacji platformy ASP.NET Core, narzędzia próby uzyskania obiektu DbContext z dostawcy usług aplikacji. Otrzymają wywołując `Program.BuildWebHost()` i uzyskiwania dostępu do `IWebHost.Services` właściwości. Wszelkie DbContext zarejestrowana przy użyciu `IServiceCollection.AddDbContext<TContext>()` należy znaleźć i utworzone w ten sposób. Ten wzorzec został [wprowadzone w programie ASP.NET 2.0 Core][1]

<a name="using-the-default-constructor"></a>Za pomocą konstruktora domyślnego
-----------------------------
Jeśli kontekstu DbContext nie można uzyskać od dostawcy usług aplikacji, narzędzia wyszukiwania dla typu DbContext wewnątrz projektu. Próbują utworzyć za pomocą jego domyślny konstruktor.

<a name="from-a-design-time-factory"></a>Z fabryki czasu projektowania
--------------------------
Możesz także wybrać narzędzia sposobu tworzenia sieci DbContext zaimplementowanie `IDesignTimeDbContextFactory`. Jeśli do klasy implementującej interfejs ten znajduje się wewnątrz projektu, narzędzia obejścia inne sposoby tworzenia kontekstu DbContext.
Fabryka oni zawsze używać w czasie projektowania. Fabryka jest szczególnie przydatne, jeśli konieczne będzie skonfigurowanie kontekstu DbContext inaczej dla czasu projektowania niż w czasie wykonywania.

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

> [!NOTE]
> `args` Parametr jest aktualnie używana. Brak [problemu] [ 2] śledzenia umożliwia określenie argumentów czasu projektowania w menu Narzędzia.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
