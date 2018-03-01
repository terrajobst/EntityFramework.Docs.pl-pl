---
title: "Uaktualnianie z programu EF Core 1.0 RC1 do RC2 — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="e77dc-102">Uaktualnianie z EF Core 1.0 RC1 do 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="e77dc-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="e77dc-103">Ten artykuł zawiera wskazówki dotyczące przenoszenia aplikacji skompilowanej za pomocą pakietów RC1 do RC2.</span><span class="sxs-lookup"><span data-stu-id="e77dc-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="e77dc-104">Pakiet nazwy i wersje</span><span class="sxs-lookup"><span data-stu-id="e77dc-104">Package Names and Versions</span></span>

<span data-ttu-id="e77dc-105">Między RC1 i RC2 możemy zmieniony z "Programu Entity Framework 7" na "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="e77dc-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="e77dc-106">Możesz przeczytać dodatkowe informacje ze względu na zmianę w [to post, w którym Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="e77dc-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="e77dc-107">Z powodu tej zmiany nazwy naszych pakietu zmieniła się z `EntityFramework.*` do `Microsoft.EntityFrameworkCore.*` i naszych w wersjach od `7.0.0-rc1-final` do `1.0.0-rc2-final` (lub `1.0.0-preview1-final` dla narzędzi).</span><span class="sxs-lookup"><span data-stu-id="e77dc-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="e77dc-108">**Konieczne będzie całkowicie Usuń pakiety RC1, a następnie zainstaluj RC2 te.**</span><span class="sxs-lookup"><span data-stu-id="e77dc-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="e77dc-109">Oto mapowania dla niektórych typowych pakietów.</span><span class="sxs-lookup"><span data-stu-id="e77dc-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="e77dc-110">Pakiet RC1</span><span class="sxs-lookup"><span data-stu-id="e77dc-110">RC1 Package</span></span>                                               | <span data-ttu-id="e77dc-111">RC2 Equivalent</span><span class="sxs-lookup"><span data-stu-id="e77dc-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="e77dc-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e77dc-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e77dc-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="e77dc-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="e77dc-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="e77dc-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="e77dc-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e77dc-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e77dc-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="e77dc-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="e77dc-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="e77dc-125">Nie są jeszcze dostępne dla RC2</span><span class="sxs-lookup"><span data-stu-id="e77dc-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="e77dc-126">EntityFramework.Commands                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="e77dc-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="e77dc-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="e77dc-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="e77dc-130">Namespaces</span><span class="sxs-lookup"><span data-stu-id="e77dc-130">Namespaces</span></span>

<span data-ttu-id="e77dc-131">Wraz z nazwy pakietu, przestrzenie nazw zmieniła się z `Microsoft.Data.Entity.*` do `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="e77dc-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="e77dc-132">Może obsłużyć tej zmiany z Znajdź/Zamień z `using Microsoft.Data.Entity` z `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e77dc-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="e77dc-133">Tabela zmian konwencji nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="e77dc-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="e77dc-134">Znaczące zmiany funkcjonalne Wybraliśmy w RC2 było użyć nazwy `DbSet<TEntity>` właściwości dla danej jednostki jako nazwy tabeli mapowania, a nie tylko nazwę klasy.</span><span class="sxs-lookup"><span data-stu-id="e77dc-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="e77dc-135">Więcej o tej zmianie w [problem pokrewne anonsu](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="e77dc-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="e77dc-136">Istniejące aplikacje RC1, zaleca się dodanie poniższego kodu do uruchomienia programu `OnModelCreating` Metoda aktualizowania strategia nazewnictwa RC1:</span><span class="sxs-lookup"><span data-stu-id="e77dc-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="e77dc-137">Jeśli chcesz przyjąć nowy strategia nazewnictwa może zalecamy pomyślnie Kończenie pozostałe kroki uaktualniania, a następnie usunięcie kodu i tworzenia migrację, aby zastosować tabelę zmienia nazwę.</span><span class="sxs-lookup"><span data-stu-id="e77dc-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="e77dc-138">AddDbContext / Startup.cs zmian (tylko dla projektów platformy ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e77dc-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e77dc-139">W RC1, trzeba było dodać usługi programu Entity Framework dla dostawcy usługi aplikacji — w `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="e77dc-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="e77dc-140">RC2, można usunąć wywołań `AddEntityFramework()`, `AddSqlServer()`itp.:</span><span class="sxs-lookup"><span data-stu-id="e77dc-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="e77dc-141">Należy również Dodaj Konstruktor do pochodnej kontekstu, która przyjmuje opcje kontekstu i przekazuje je do podstawowego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e77dc-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="e77dc-142">Jest to niezbędne, ponieważ firma Microsoft usunęła niektóre magic przerażenie, który je w snuck w tle:</span><span class="sxs-lookup"><span data-stu-id="e77dc-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="e77dc-143">W interfejsie IServiceProvider może być przekazywany</span><span class="sxs-lookup"><span data-stu-id="e77dc-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="e77dc-144">Kod RC1, która przekazuje `IServiceProvider` do kontekstu, to teraz przeniesiono do `DbContextOptions`, a parametr oddzielne konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e77dc-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="e77dc-145">Użyj `DbContextOptionsBuilder.UseInternalServiceProvider(...)` można ustawić dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="e77dc-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="e77dc-146">Testowanie</span><span class="sxs-lookup"><span data-stu-id="e77dc-146">Testing</span></span>

<span data-ttu-id="e77dc-147">Najbardziej typowym scenariuszem dla tej czynności było kontrolowanie zakresu InMemory bazy danych podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="e77dc-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="e77dc-148">Zobacz zaktualizowanego [testowanie](testing/index.md) artykule przykład temu RC2.</span><span class="sxs-lookup"><span data-stu-id="e77dc-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="e77dc-149">Rozpoznawanie wewnętrznych usług od dostawcy usług aplikacji (tylko dla projektów platformy ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e77dc-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e77dc-150">Jeśli korzystasz z aplikacji platformy ASP.NET Core i chcesz EF do rozpoznania usług wewnętrznego od dostawcy usług aplikacji, jest przeciążenie metody `AddDbContext` która pozwala na skonfigurowanie to:</span><span class="sxs-lookup"><span data-stu-id="e77dc-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="e77dc-151">Firma Microsoft zaleca stosowanie EF wewnętrznie Zarządzanie usługach, chyba że przyczyny, aby połączyć wewnętrznych usług EF w aplikacji usługodawcy.</span><span class="sxs-lookup"><span data-stu-id="e77dc-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="e77dc-152">Głównym celem się, że chcesz to zrobić jest na potrzeby Zastąp usług, które jest używane wewnętrznie EF dostawcą usług aplikacji</span><span class="sxs-lookup"><span data-stu-id="e77dc-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="e77dc-153">Polecenia DNX = > .NET interfejsu wiersza polecenia (tylko dla projektów platformy ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="e77dc-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="e77dc-154">Jeśli poprzednio korzystano `dnx ef` poleceń dla projektów ASP.NET 5, te zostały przeniesione do `dotnet ef` poleceń.</span><span class="sxs-lookup"><span data-stu-id="e77dc-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="e77dc-155">Taka sama składnia polecenia nadal istnieje.</span><span class="sxs-lookup"><span data-stu-id="e77dc-155">The same command syntax still applies.</span></span> <span data-ttu-id="e77dc-156">Można użyć `dotnet ef --help` dla informacji o składni.</span><span class="sxs-lookup"><span data-stu-id="e77dc-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="e77dc-157">Sposób zarejestrowanych poleceń została zmieniona w RC2 z powodu DNX zastępowane przez .NET interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="e77dc-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="e77dc-158">Polecenia są obecnie zarejestrowane w `tools` sekcji `project.json`:</span><span class="sxs-lookup"><span data-stu-id="e77dc-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="e77dc-159">Jeśli używasz programu Visual Studio, można teraz używać konsoli Menedżera pakietów do uruchomienia poleceń EF dla projektów platformy ASP.NET Core (to nie jest obsługiwana w RC1).</span><span class="sxs-lookup"><span data-stu-id="e77dc-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="e77dc-160">Nadal musisz zarejestrować polecenia w `tools` sekcji `project.json` w tym celu.</span><span class="sxs-lookup"><span data-stu-id="e77dc-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="e77dc-161">Menedżer pakietów polecenia wymagają programu PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="e77dc-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="e77dc-162">Użycie poleceń programu Entity Framework w konsoli Menedżera pakietów w programie Visual Studio, następnie należy upewnić się, że zainstalowana 5 programu PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e77dc-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="e77dc-163">Jest to wymagane tymczasowe, które zostaną usunięte w następnej wersji (zobacz [wystawiać #5327](https://github.com/aspnet/EntityFramework/issues/5327) więcej szczegółów).</span><span class="sxs-lookup"><span data-stu-id="e77dc-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="e77dc-164">Za pomocą "imports" w pliku project.json</span><span class="sxs-lookup"><span data-stu-id="e77dc-164">Using "imports" in project.json</span></span>

<span data-ttu-id="e77dc-165">Część EF Core zależności nie obsługują .NET Standard jeszcze.</span><span class="sxs-lookup"><span data-stu-id="e77dc-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="e77dc-166">Podstawowe EF w projektach platformy .NET Standard i .NET Core mogą wymagać dodawanie "imports" do pliku project.json tymczasowo.</span><span class="sxs-lookup"><span data-stu-id="e77dc-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="e77dc-167">Podczas dodawania EF, przywracanie NuGet spowoduje wyświetlenie tego komunikatu o błędzie:</span><span class="sxs-lookup"><span data-stu-id="e77dc-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="e77dc-168">Obejście jest ręcznie zaimportować profil przenośnych "portable net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="e77dc-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="e77dc-169">To wymusza NuGet traktować to pliki binarne spełniające to Podaj jako zgodne framework przy użyciu platformy .NET Standard, nawet jeśli nie są one.</span><span class="sxs-lookup"><span data-stu-id="e77dc-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="e77dc-170">Mimo że "portable net451 + win8" nie jest zgodny z .NET Standard 100%, jest zgodna wystarczająco przejście od PCL do .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e77dc-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="e77dc-171">Importy można usunąć zależności w EF po pewnym czasie uaktualnić do wersji .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="e77dc-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="e77dc-172">Wiele platform można dodać do "imports" w składni tablicy.</span><span class="sxs-lookup"><span data-stu-id="e77dc-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="e77dc-173">Inne importów może być konieczne, jeśli Dodawanie dodatkowych bibliotek do projektu.</span><span class="sxs-lookup"><span data-stu-id="e77dc-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="e77dc-174">Zobacz [wystawiać #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="e77dc-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
