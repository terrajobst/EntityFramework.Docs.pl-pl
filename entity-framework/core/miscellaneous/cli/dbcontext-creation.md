---
title: Tworzenie DbContext w czasie projektowania — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655523"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="da24a-102">Tworzenie klasy DbContext w czasie projektowania</span><span class="sxs-lookup"><span data-stu-id="da24a-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="da24a-103">Niektóre polecenia narzędzi EF Core (na przykład polecenia [migracji][1] ) wymagają utworzenia wystąpienia pochodnego `DbContext`, które ma zostać utworzone w czasie projektowania w celu zebrania szczegółowych informacji o typach jednostek aplikacji i sposobie ich mapowania na schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="da24a-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="da24a-104">W większości przypadków wskazane jest, aby `DbContext` utworzone w ten sposób został skonfigurowany w podobny sposób, jak zostanie [skonfigurowany w czasie wykonywania][2].</span><span class="sxs-lookup"><span data-stu-id="da24a-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="da24a-105">Istnieją różne sposoby tworzenia `DbContext`przez narzędzia:</span><span class="sxs-lookup"><span data-stu-id="da24a-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="da24a-106">Z usług aplikacji</span><span class="sxs-lookup"><span data-stu-id="da24a-106">From application services</span></span>

<span data-ttu-id="da24a-107">Jeśli projekt startowy używa hosta [sieci Web ASP.NET Core][3] lub [hosta ogólnego platformy .NET Core][4], narzędzia próbują uzyskać obiekt DbContext od dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da24a-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="da24a-108">Narzędzia najpierw próbują uzyskać dostawcę usługi przez wywoływanie `Program.CreateHostBuilder()`, wywoływanie `Build()`, a następnie uzyskanie dostępu do właściwości `Services`.</span><span class="sxs-lookup"><span data-stu-id="da24a-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="da24a-109">Podczas tworzenia nowej aplikacji ASP.NET Core ten element Hook jest uwzględniany domyślnie.</span><span class="sxs-lookup"><span data-stu-id="da24a-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="da24a-110">Sama `DbContext` i wszelkie zależności w jego konstruktorze muszą być zarejestrowane jako usługi w dostawcy usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da24a-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="da24a-111">Można to łatwo osiągnąć poprzez posiadanie [konstruktora na `DbContext`, który przyjmuje wystąpienie `DbContextOptions<TContext>` jako argument][5] i przy użyciu [metody`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="da24a-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="da24a-112">Korzystanie z konstruktora bez parametrów</span><span class="sxs-lookup"><span data-stu-id="da24a-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="da24a-113">Jeśli nie można uzyskać kontekstu DbContext od dostawcy usług aplikacji, narzędzia szukają typu pochodnego `DbContext` wewnątrz projektu.</span><span class="sxs-lookup"><span data-stu-id="da24a-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="da24a-114">Następnie próbują utworzyć wystąpienie przy użyciu konstruktora bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="da24a-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="da24a-115">Może to być Konstruktor domyślny, jeśli `DbContext` jest skonfigurowany przy użyciu metody [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="da24a-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="da24a-116">Z fabryki czasu projektowania</span><span class="sxs-lookup"><span data-stu-id="da24a-116">From a design-time factory</span></span>

<span data-ttu-id="da24a-117">Możesz również poinformować narzędzia, jak utworzyć DbContext, implementując interfejs `IDesignTimeDbContextFactory<TContext>`: Jeśli klasa implementująca ten interfejs znajduje się w tym samym projekcie, co pochodna `DbContext` lub w projekcie startowym aplikacji, narzędzia pomijają pozostałe sposoby tworzenia kontekstu DBI używania fabryki czasu projektowania.</span><span class="sxs-lookup"><span data-stu-id="da24a-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="da24a-118">Parametr `args` nie jest obecnie używany.</span><span class="sxs-lookup"><span data-stu-id="da24a-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="da24a-119">Występuje [problem][8] ze śledzeniem możliwości określania argumentów czasu projektowania z narzędzi.</span><span class="sxs-lookup"><span data-stu-id="da24a-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="da24a-120">Fabryka czasu projektowania może być szczególnie przydatna, jeśli trzeba skonfigurować DbContext dla czasu projektowania niż w czasie wykonywania, jeśli Konstruktor `DbContext` przyjmuje dodatkowe parametry nie są zarejestrowane w programie DI, jeśli nie używasz DI na pewno lub z jakiegoś powodu wolisz, aby nie mieć metody `BuildWebHost` w klasie `Main` aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da24a-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
