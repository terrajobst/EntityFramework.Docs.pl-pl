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
ms.locfileid: "30202487"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="8df48-102">Tworzenie typu DbContext w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="8df48-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="8df48-103">Niektóre polecenia EF podstawowe narzędzia (na przykład [migracje] [ 1] poleceń) wymagają pochodnego `DbContext` wystąpienia należy utworzyć w czasie projektowania w celu zbierania informacji o aplikacji typy jednostek oraz sposobu mapowania ich na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8df48-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="8df48-104">W większości przypadków jest pożądane który `DbContext` utworzone w ten sposób jest skonfigurowany w sposób podobny do sposobu byłoby [skonfigurowane w czasie wykonywania][2].</span><span class="sxs-lookup"><span data-stu-id="8df48-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="8df48-105">Istnieją różne sposoby, spróbuj utworzyć narzędzia `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="8df48-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="8df48-106">Z usługi aplikacji</span><span class="sxs-lookup"><span data-stu-id="8df48-106">From application services</span></span>
-------------------------
<span data-ttu-id="8df48-107">Twój projekt startowy w przypadku aplikacji platformy ASP.NET Core, narzędzia próby uzyskania obiektu DbContext z dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8df48-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="8df48-108">Narzędzie najpierw spróbuj uzyskać dostawcę usługi za pomocą `Program.BuildWebHost()` i uzyskiwania dostępu do `IWebHost.Services` właściwości.</span><span class="sxs-lookup"><span data-stu-id="8df48-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="8df48-109">Podczas tworzenia nowej aplikacji ASP.NET Core 2.0 domyślnie znajduje się ta punktu zaczepienia.</span><span class="sxs-lookup"><span data-stu-id="8df48-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="8df48-110">W poprzednich wersjach programu EF Core i ASP.NET Core, narzędzia próbuj wywoływać `Startup.ConfigureServices` bezpośrednio w celu uzyskania dostawcy usług aplikacji, ale ten wzorzec już nie działa prawidłowo w aplikacjach ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="8df48-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="8df48-111">Uaktualniania aplikacji platformy ASP.NET Core 1.x 2.0, można [zmodyfikować Twoje `Program` klasy do nowego wzorcem][3].</span><span class="sxs-lookup"><span data-stu-id="8df48-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="8df48-112">`DbContext` Się i zależności w swoich konstruktorach należy zarejestrować jako usługi w aplikacji usługodawcy.</span><span class="sxs-lookup"><span data-stu-id="8df48-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="8df48-113">Można to łatwo osiągnąć dzięki użyciu [Konstruktor na `DbContext` pobierającej wystąpienia `DbContextOptions<TContext>` jako argument] [ 4] i przy użyciu [ `AddDbContext<TContext>` —Metoda] [5].</span><span class="sxs-lookup"><span data-stu-id="8df48-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="8df48-114">Za pomocą konstruktora bez parametrów</span><span class="sxs-lookup"><span data-stu-id="8df48-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="8df48-115">Jeśli DbContext nie można uzyskać od dostawcy usług aplikacji, wyszukaj narzędzia pochodnej `DbContext` typ wewnątrz projektu.</span><span class="sxs-lookup"><span data-stu-id="8df48-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="8df48-116">Następnie próby utworzenia wystąpienia przy użyciu konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="8df48-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="8df48-117">Może to być domyślnego konstruktora, jeśli `DbContext` jest konfigurowana przy użyciu [ `OnConfiguring` ] [ 6] metody.</span><span class="sxs-lookup"><span data-stu-id="8df48-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="8df48-118">Z fabryki czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="8df48-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="8df48-119">Możesz także wybrać narzędzia sposobu tworzenia sieci DbContext zaimplementowanie `IDesignTimeDbContextFactory<TContext>` interfejsu: Jeśli klasy implementującej interfejs ten znajduje się w jednym tego samego projektu jako pochodnej `DbContext` lub w aplikacji projekt startowy, obejście narzędzia inne metody tworzenia zamiast tego obiektu DbContext i używania fabryki czasu projektowania.</span><span class="sxs-lookup"><span data-stu-id="8df48-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="8df48-120">`args` Parametr jest aktualnie używana.</span><span class="sxs-lookup"><span data-stu-id="8df48-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="8df48-121">Brak [problemu] [ 7] śledzenia umożliwia określenie argumentów czasu projektowania w menu Narzędzia.</span><span class="sxs-lookup"><span data-stu-id="8df48-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="8df48-122">Fabryka czasu projektowania może być szczególnie przydatne, jeśli konieczne będzie skonfigurowanie kontekstu DbContext inaczej dla czasu projektowania niż w czasie wykonywania, jeśli `DbContext` Konstruktor ma dodatkowe parametry nie są zarejestrowane w Podpisane, jeśli nie używasz Podpisane na wszystkich lub, jeśli dla niektórych przyczyny nie chcesz mieć `BuildWebHost` metody w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8df48-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="8df48-123">`Main` Klasa.</span><span class="sxs-lookup"><span data-stu-id="8df48-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
