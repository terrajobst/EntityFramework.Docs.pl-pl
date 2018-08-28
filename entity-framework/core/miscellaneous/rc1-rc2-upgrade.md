---
title: Uaktualnianie z programu EF Core 1.0 RC1 do RC2 — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996900"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="83bdf-102">Uaktualnianie z programu EF Core 1.0 RC1 do wersji 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="83bdf-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="83bdf-103">Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów RC1 do RC2.</span><span class="sxs-lookup"><span data-stu-id="83bdf-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="83bdf-104">Nazwy pakietów i wersje</span><span class="sxs-lookup"><span data-stu-id="83bdf-104">Package Names and Versions</span></span>

<span data-ttu-id="83bdf-105">Między RC1 i RC2 firma Microsoft zmieniony z "Entity Framework 7" na "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="83bdf-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="83bdf-106">Możesz dowiedzieć się więcej o ze względu na zmianę w [ten wpis, Scott hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="83bdf-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="83bdf-107">Ze względu na tę zmianę nazw naszych pakietów zmieniła się z `EntityFramework.*` do `Microsoft.EntityFrameworkCore.*` i nasze w wersjach od `7.0.0-rc1-final` do `1.0.0-rc2-final` (lub `1.0.0-preview1-final` narzędzi).</span><span class="sxs-lookup"><span data-stu-id="83bdf-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="83bdf-108">**Konieczne będzie całkowicie usunąć pakiety RC1, a następnie zainstaluj RC2 te.**</span><span class="sxs-lookup"><span data-stu-id="83bdf-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="83bdf-109">Poniżej przedstawiono mapowanie niektóre popularne pakiety innych.</span><span class="sxs-lookup"><span data-stu-id="83bdf-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="83bdf-110">Pakiet RC1</span><span class="sxs-lookup"><span data-stu-id="83bdf-110">RC1 Package</span></span>                                               | <span data-ttu-id="83bdf-111">Odpowiednik RC2</span><span class="sxs-lookup"><span data-stu-id="83bdf-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="83bdf-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="83bdf-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="83bdf-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="83bdf-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="83bdf-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="83bdf-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="83bdf-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="83bdf-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="83bdf-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="83bdf-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="83bdf-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="83bdf-125">Nie jest jeszcze dostępna RC2</span><span class="sxs-lookup"><span data-stu-id="83bdf-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="83bdf-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="83bdf-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="83bdf-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="83bdf-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="83bdf-130">Namespaces</span><span class="sxs-lookup"><span data-stu-id="83bdf-130">Namespaces</span></span>

<span data-ttu-id="83bdf-131">Wraz z nazwy pakietu, przestrzenie nazw zmieniła się z `Microsoft.Data.Entity.*` do `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="83bdf-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="83bdf-132">Może obsłużyć tej zmiany z Znajdź/Zamień z `using Microsoft.Data.Entity` z `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="83bdf-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="83bdf-133">Tabela zmian konwencji nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="83bdf-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="83bdf-134">Znaczące zmiany funkcjonalne w wersji RC2, Skorzystaliśmy było użyć nazwy `DbSet<TEntity>` właściwości dla danej jednostki, jako nazwę tabeli mapowania, a nie tylko nazwę klasy.</span><span class="sxs-lookup"><span data-stu-id="83bdf-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="83bdf-135">Możesz dowiedzieć się więcej o tej zmianie w [problem powiązane ogłoszenie](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="83bdf-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="83bdf-136">W przypadku istniejących aplikacji RC1, firma Microsoft zaleca, dodając następujący kod na początku swoje `OnModelCreating` metodę, aby zachować strategia nazewnictwa RC1:</span><span class="sxs-lookup"><span data-stu-id="83bdf-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="83bdf-137">Jeśli chcesz, który wdrożył nowy strategia nazewnictwa, czy zalecane jest pomyślnie Kończenie pozostałe kroki uaktualnienia, a następnie usunięcie kodu i tworzenia migracji do zastosowania w tabeli zmienia nazwę.</span><span class="sxs-lookup"><span data-stu-id="83bdf-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="83bdf-138">AddDbContext / Startup.cs zmian (tylko w przypadku projektów ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="83bdf-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="83bdf-139">W wersji RC1, trzeba było dodać usługi Entity Framework do dostawcy usług w aplikacji — w `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="83bdf-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="83bdf-140">W wersji RC2, możesz usunąć wywołania `AddEntityFramework()`, `AddSqlServer()`itp.:</span><span class="sxs-lookup"><span data-stu-id="83bdf-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="83bdf-141">Musisz również dodać Konstruktor pochodnej kontekst, który przyjmuje opcje kontekstu i przekazuje je do konstruktora podstawowego.</span><span class="sxs-lookup"><span data-stu-id="83bdf-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="83bdf-142">Jest to niezbędne, ponieważ firma Microsoft usunęła niektóre scary magic, który snuck je w tle:</span><span class="sxs-lookup"><span data-stu-id="83bdf-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="83bdf-143">Przekazywanie w IServiceProvider.</span><span class="sxs-lookup"><span data-stu-id="83bdf-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="83bdf-144">Jeśli masz kod RC1, który przekazuje `IServiceProvider` kontekst, to teraz przeniesiono do `DbContextOptions`, a nie parametrem oddzielne konstruktora.</span><span class="sxs-lookup"><span data-stu-id="83bdf-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="83bdf-145">Użyj `DbContextOptionsBuilder.UseInternalServiceProvider(...)` można ustawić dostawcę usług.</span><span class="sxs-lookup"><span data-stu-id="83bdf-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="83bdf-146">Testowanie</span><span class="sxs-lookup"><span data-stu-id="83bdf-146">Testing</span></span>

<span data-ttu-id="83bdf-147">Najbardziej typowym scenariuszem tego zrobić był umożliwiają kontrolowanie zakresu InMemory bazy danych, podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="83bdf-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="83bdf-148">Zobacz zaktualizowanego [testowania](testing/index.md) artykułu, na przykład w ten sposób za pomocą RC2.</span><span class="sxs-lookup"><span data-stu-id="83bdf-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="83bdf-149">Rozpoznawanie wewnętrznych usług od dostawcy usług w aplikacji (tylko w przypadku projektów ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="83bdf-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="83bdf-150">Aplikacja ASP.NET Core, i chcesz EF, aby rozwiązać wewnętrznych usług od dostawcy usług w aplikacji, czy przeciążenia `AddDbContext` pozwala skonfigurować to:</span><span class="sxs-lookup"><span data-stu-id="83bdf-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="83bdf-151">Firma Microsoft zaleca, dzięki czemu EF wewnętrznie Zarządzanie własnych usług, chyba że masz powód, aby połączyć wewnętrznych usług EF w aplikacji dostawcy usługi.</span><span class="sxs-lookup"><span data-stu-id="83bdf-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="83bdf-152">Głównym powodem, czy chcesz to zrobić jest Zastąp usług, które jest używane wewnętrznie EF za pomocą dostawcy usługi aplikacji</span><span class="sxs-lookup"><span data-stu-id="83bdf-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="83bdf-153">Poleceń środowiska DNX = > .NET interfejsu wiersza polecenia (tylko w przypadku projektów ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="83bdf-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="83bdf-154">Jeśli wcześniej używano `dnx ef` poleceń dla projektów ASP.NET 5, te zostały przeniesione do `dotnet ef` poleceń.</span><span class="sxs-lookup"><span data-stu-id="83bdf-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="83bdf-155">Tej samej składni polecenia nadal obowiązuje ograniczenie.</span><span class="sxs-lookup"><span data-stu-id="83bdf-155">The same command syntax still applies.</span></span> <span data-ttu-id="83bdf-156">Możesz użyć `dotnet ef --help` Aby uzyskać informacje o składni.</span><span class="sxs-lookup"><span data-stu-id="83bdf-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="83bdf-157">Sposób, w jaki są rejestrowane polecenia został zmieniony w wersji RC2 z powodu środowiska DNX, jest zastępowany przez interfejs wiersza polecenia platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="83bdf-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="83bdf-158">Polecenia są teraz zarejestrowane w usłudze `tools` sekcji `project.json`:</span><span class="sxs-lookup"><span data-stu-id="83bdf-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="83bdf-159">Jeśli używasz programu Visual Studio, możesz teraz używać Konsola Menedżera pakietów do uruchamiania poleceń EF dla projektów ASP.NET Core (to zostało nie obsługiwane w wersji RC1).</span><span class="sxs-lookup"><span data-stu-id="83bdf-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="83bdf-160">Nadal trzeba zarejestrować poleceń w `tools` części `project.json` w tym celu.</span><span class="sxs-lookup"><span data-stu-id="83bdf-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="83bdf-161">Menedżer pakietów polecenia wymagają programu PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="83bdf-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="83bdf-162">Jeśli używasz poleceń programu Entity Framework w konsoli Menedżera pakietów w programie Visual Studio, następnie należy upewnić się, że masz zainstalowanego 5 programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="83bdf-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="83bdf-163">Jest to wymagane tymczasowe, która zostanie usunięta w następnej wersji (zobacz [wystawiać #5327](https://github.com/aspnet/EntityFramework/issues/5327) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="83bdf-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="83bdf-164">Za pomocą "import" w pliku project.json</span><span class="sxs-lookup"><span data-stu-id="83bdf-164">Using "imports" in project.json</span></span>

<span data-ttu-id="83bdf-165">Niektóre z programem EF Core zależności nie obsługują .NET Standard jeszcze.</span><span class="sxs-lookup"><span data-stu-id="83bdf-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="83bdf-166">EF Core w projektach .NET Standard i .NET Core może wymagać, dodając "imports" do pliku project.json jako rozwiązanie tymczasowe.</span><span class="sxs-lookup"><span data-stu-id="83bdf-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="83bdf-167">Podczas dodawania EF, przywracanie pakietów NuGet spowoduje wyświetlenie tego komunikatu o błędzie:</span><span class="sxs-lookup"><span data-stu-id="83bdf-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="83bdf-168">Obejście polega na ręcznie zaimportować przenośnej profilu "portable net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="83bdf-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="83bdf-169">Tego pakietu NuGet, aby traktować to pliki binarne, które odpowiadają tym wymusza działają jako środowisko zgodne z technologią .NET Standard, nawet jeśli nie są one.</span><span class="sxs-lookup"><span data-stu-id="83bdf-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="83bdf-170">Mimo że "portable net451 + win8" nie jest zgodny z .NET Standard w 100%, są one zgodne, przejścia od PCL .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="83bdf-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="83bdf-171">Importy można usunąć zależności EF firmy po pewnym czasie uaktualnić do wersji .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="83bdf-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="83bdf-172">Do "import" można dodać wielu platform przy użyciu składni tablicy.</span><span class="sxs-lookup"><span data-stu-id="83bdf-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="83bdf-173">Pozostałe Importy może być konieczne, jeśli dodatkowe biblioteki zostaną dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="83bdf-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="83bdf-174">Zobacz [wystawiać #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="83bdf-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
