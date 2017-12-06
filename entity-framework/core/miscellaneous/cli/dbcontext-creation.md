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
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="4204e-102">Tworzenie typu DbContext w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="4204e-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="4204e-103">Niektóre narzędzia EF polecenia wymagają wystąpienie typu DbContext ma zostać utworzony na projekt czasu (na przykład podczas uruchamiania polecenia migracji).</span><span class="sxs-lookup"><span data-stu-id="4204e-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="4204e-104">Istnieją różne sposoby narzędzia spróbuj utworzyć ją.</span><span class="sxs-lookup"><span data-stu-id="4204e-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="4204e-105">Z usługi aplikacji</span><span class="sxs-lookup"><span data-stu-id="4204e-105">From application services</span></span>
-------------------------
<span data-ttu-id="4204e-106">Twój projekt startowy w przypadku aplikacji platformy ASP.NET Core, narzędzia próby uzyskania obiektu DbContext z dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4204e-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="4204e-107">Otrzymają wywołując `Program.BuildWebHost()` i uzyskiwania dostępu do `IWebHost.Services` właściwości.</span><span class="sxs-lookup"><span data-stu-id="4204e-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="4204e-108">Wszelkie DbContext zarejestrowana przy użyciu `IServiceCollection.AddDbContext<TContext>()` należy znaleźć i utworzone w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="4204e-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="4204e-109">Ten wzorzec został [wprowadzone w programie ASP.NET 2.0 Core][1]</span><span class="sxs-lookup"><span data-stu-id="4204e-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="4204e-110">Za pomocą konstruktora domyślnego</span><span class="sxs-lookup"><span data-stu-id="4204e-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="4204e-111">Jeśli kontekstu DbContext nie można uzyskać od dostawcy usług aplikacji, narzędzia wyszukiwania dla typu DbContext wewnątrz projektu.</span><span class="sxs-lookup"><span data-stu-id="4204e-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="4204e-112">Próbują utworzyć za pomocą jego domyślny konstruktor.</span><span class="sxs-lookup"><span data-stu-id="4204e-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="4204e-113">Z fabryki czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="4204e-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="4204e-114">Możesz także wybrać narzędzia sposobu tworzenia sieci DbContext zaimplementowanie `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="4204e-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="4204e-115">Jeśli do klasy implementującej interfejs ten znajduje się wewnątrz projektu, narzędzia obejścia inne sposoby tworzenia kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="4204e-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="4204e-116">Fabryka oni zawsze używać w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="4204e-116">They always use the factory at design time.</span></span> <span data-ttu-id="4204e-117">Fabryka jest szczególnie przydatne, jeśli konieczne będzie skonfigurowanie kontekstu DbContext inaczej dla czasu projektowania niż w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="4204e-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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
> <span data-ttu-id="4204e-118">`args` Parametr jest aktualnie używana.</span><span class="sxs-lookup"><span data-stu-id="4204e-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="4204e-119">Brak [problemu] [ 2] śledzenia umożliwia określenie argumentów czasu projektowania w menu Narzędzia.</span><span class="sxs-lookup"><span data-stu-id="4204e-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
