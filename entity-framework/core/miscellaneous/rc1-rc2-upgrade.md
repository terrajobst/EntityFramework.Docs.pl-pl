---
title: Uaktualnianie z wersji EF Core 1,0 RC1 do RC2 — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416611"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="5be81-102">Uaktualnianie z wersji EF Core 1,0 RC1 do 1,0 RC2</span><span class="sxs-lookup"><span data-stu-id="5be81-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="5be81-103">Ten artykuł zawiera wskazówki dotyczące przesuwania aplikacji skompilowanej z pakietami RC1 do RC2.</span><span class="sxs-lookup"><span data-stu-id="5be81-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="5be81-104">Nazwy i wersje pakietów</span><span class="sxs-lookup"><span data-stu-id="5be81-104">Package Names and Versions</span></span>

<span data-ttu-id="5be81-105">Między RC1 a RC2 zmieniono z "Entity Framework 7" na "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="5be81-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="5be81-106">Więcej informacji na temat przyczyn zmiany w [tym wpisie można znaleźć Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="5be81-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="5be81-107">Ze względu na tę zmianę nazwy pakietów zmieniły się z `EntityFramework.*` na `Microsoft.EntityFrameworkCore.*` i naszych wersji z `7.0.0-rc1-final` do `1.0.0-rc2-final` (lub `1.0.0-preview1-final` do narzędzia).</span><span class="sxs-lookup"><span data-stu-id="5be81-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="5be81-108">**Konieczne będzie całkowite usunięcie pakietów RC1, a następnie zainstalowanie programu RC2.**</span><span class="sxs-lookup"><span data-stu-id="5be81-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="5be81-109">Oto mapowanie niektórych typowych pakietów.</span><span class="sxs-lookup"><span data-stu-id="5be81-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="5be81-110">Pakiet RC1</span><span class="sxs-lookup"><span data-stu-id="5be81-110">RC1 Package</span></span>                                               | <span data-ttu-id="5be81-111">Równoważność RC2</span><span class="sxs-lookup"><span data-stu-id="5be81-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="5be81-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5be81-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5be81-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5be81-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5be81-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5be81-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5be81-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="5be81-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="5be81-117">NpgSql. EntityFrameworkCore. Postgres <to be advised></span><span class="sxs-lookup"><span data-stu-id="5be81-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="5be81-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5be81-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5be81-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5be81-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5be81-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5be81-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5be81-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5be81-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5be81-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="5be81-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="5be81-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="5be81-125">Jeszcze niedostępne dla wersji RC2</span><span class="sxs-lookup"><span data-stu-id="5be81-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="5be81-126">EntityFramework. Commands 7.0.0-RC1 — Final</span><span class="sxs-lookup"><span data-stu-id="5be81-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-127">Microsoft. EntityFrameworkCore. Tools 1.0.0-zestawu — wersja finalna</span><span class="sxs-lookup"><span data-stu-id="5be81-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="5be81-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="5be81-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="5be81-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="5be81-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="5be81-130">Przestrzenie nazw</span><span class="sxs-lookup"><span data-stu-id="5be81-130">Namespaces</span></span>

<span data-ttu-id="5be81-131">Wraz z nazwami pakietów przestrzenie nazw zmieniają się z `Microsoft.Data.Entity.*` na `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="5be81-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="5be81-132">Tę zmianę można obsłużyć za pomocą Znajdź/Zamień `using Microsoft.Data.Entity` z `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="5be81-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="5be81-133">Zmiany konwencji nazewnictwa tabel</span><span class="sxs-lookup"><span data-stu-id="5be81-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="5be81-134">Istotna zmiana funkcjonalna w wersji RC2 była używana jako nazwa właściwości `DbSet<TEntity>` danej jednostki jako nazwy tabeli mapowanej na, a nie tylko nazwa klasy.</span><span class="sxs-lookup"><span data-stu-id="5be81-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="5be81-135">Więcej informacji na temat tej zmiany można znaleźć w [powiązanym problemie związanym z ogłoszeniem](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="5be81-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="5be81-136">W przypadku istniejących aplikacji w wersji RC1 zalecamy dodanie następującego kodu do początku metody `OnModelCreating`, aby zachować strategię nazewnictwa RC1:</span><span class="sxs-lookup"><span data-stu-id="5be81-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="5be81-137">Jeśli chcesz przyjąć nową strategię nazewnictwa, zalecamy pomyślne zakończenie pozostałej części kroków uaktualnienia, a następnie usunięcie kodu i utworzenie migracji w celu zastosowania zmian nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="5be81-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="5be81-138">AddDbContext/Startup.cs zmiany (tylko projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="5be81-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="5be81-139">W wersji RC1 należy dodać usługi Entity Framework do dostawcy usługi aplikacji — w `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="5be81-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="5be81-140">W wersji RC2 można usunąć wywołania do `AddEntityFramework()`, `AddSqlServer()`itp.:</span><span class="sxs-lookup"><span data-stu-id="5be81-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="5be81-141">Należy również dodać konstruktora do kontekstu pochodnego, który przyjmuje opcje kontekstu i przekazuje je do konstruktora podstawowego.</span><span class="sxs-lookup"><span data-stu-id="5be81-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="5be81-142">Jest to potrzebne, ponieważ usunęliśmy część Scary Magic, która snuck je w tle:</span><span class="sxs-lookup"><span data-stu-id="5be81-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="5be81-143">Przekazywanie w IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="5be81-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="5be81-144">Jeśli masz kod w wersji RC1, który przekazuje `IServiceProvider` do kontekstu, został on teraz przeniesiony do `DbContextOptions`, a nie jako osobny parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5be81-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="5be81-145">Użyj `DbContextOptionsBuilder.UseInternalServiceProvider(...)`, aby ustawić dostawcę usług.</span><span class="sxs-lookup"><span data-stu-id="5be81-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="5be81-146">Testowanie</span><span class="sxs-lookup"><span data-stu-id="5be81-146">Testing</span></span>

<span data-ttu-id="5be81-147">Najbardziej typowym scenariuszem tego jest kontrolowanie zakresu bazy danych inMemory podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="5be81-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="5be81-148">Zapoznaj się z zaktualizowanym artykułem dotyczącym [testowania](testing/index.md) , aby zapoznać się z przykładem w wersji RC2.</span><span class="sxs-lookup"><span data-stu-id="5be81-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="5be81-149">Rozpoznawanie wewnętrznych usług z poziomu dostawcy usługi aplikacji (tylko projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="5be81-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="5be81-150">Jeśli masz aplikację ASP.NET Core i chcesz, aby program Dr mógł rozpoznać usługi wewnętrzne od dostawcy usług aplikacji, istnieje Przeciążenie `AddDbContext`, które umożliwia skonfigurowanie tego elementu:</span><span class="sxs-lookup"><span data-stu-id="5be81-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="5be81-151">Zalecamy umożliwienie wewnętrznie zarządzania własnymi usługami przez program EF, chyba że masz powód, aby połączyć wewnętrzne usługi EF z dostawcą usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5be81-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="5be81-152">Głównym powodem może być użycie dostawcy usług aplikacji w celu zastąpienia usług używanych wewnętrznie przez program Dr</span><span class="sxs-lookup"><span data-stu-id="5be81-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="5be81-153">ŚRODOWISKA DNX Commands > = Interfejs wiersza polecenia platformy .NET (tylko projekty ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="5be81-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="5be81-154">Jeśli wcześniej użyto poleceń `dnx ef` dla projektów ASP.NET 5, zostały one przeniesione do `dotnet ef` poleceń.</span><span class="sxs-lookup"><span data-stu-id="5be81-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="5be81-155">Nadal stosuje się tę samą składnię polecenia.</span><span class="sxs-lookup"><span data-stu-id="5be81-155">The same command syntax still applies.</span></span> <span data-ttu-id="5be81-156">`dotnet ef --help` można użyć do informacji o składni.</span><span class="sxs-lookup"><span data-stu-id="5be81-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="5be81-157">Sposób rejestrowania poleceń został zmieniony w RC2, z powodu zamienienia środowiska DNX przez interfejs wiersza polecenia platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="5be81-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="5be81-158">Polecenia są teraz zarejestrowane w sekcji `tools` w `project.json`:</span><span class="sxs-lookup"><span data-stu-id="5be81-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="5be81-159">W przypadku korzystania z programu Visual Studio można teraz używać konsoli Menedżera pakietów do uruchamiania poleceń EF dla projektów ASP.NET Core (nie jest to obsługiwane w wersji RC1).</span><span class="sxs-lookup"><span data-stu-id="5be81-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="5be81-160">W tym celu należy zarejestrować polecenia w sekcji `tools` `project.json`.</span><span class="sxs-lookup"><span data-stu-id="5be81-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="5be81-161">Polecenia Menedżera pakietów wymagają programu PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="5be81-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="5be81-162">Jeśli używasz poleceń Entity Framework w konsoli Menedżera pakietów w programie Visual Studio, musisz upewnić się, że jest zainstalowany program PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="5be81-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="5be81-163">Jest to tymczasowy wymóg, który zostanie usunięty w następnej wersji (zobacz [problem #5327](https://github.com/aspnet/EntityFramework/issues/5327) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="5be81-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="5be81-164">Używanie "Imports" w pliku Project. JSON</span><span class="sxs-lookup"><span data-stu-id="5be81-164">Using "imports" in project.json</span></span>

<span data-ttu-id="5be81-165">Niektóre zależności EF Core nie obsługują jeszcze .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="5be81-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="5be81-166">EF Core w projektach .NET Standard i .NET Core mogą wymagać dodania elementu "Imports" do pliku Project. JSON jako tymczasowego obejścia.</span><span class="sxs-lookup"><span data-stu-id="5be81-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="5be81-167">Po dodaniu EF, w wyniku przywracania NuGet zostanie wyświetlony następujący komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="5be81-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="5be81-168">Obejście polega na ręcznym zaimportowaniu przenośnego profilu "Portable-net451 + Win8".</span><span class="sxs-lookup"><span data-stu-id="5be81-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="5be81-169">Wymusza to, aby pakiet NuGet traktował te dane binarne, które pasują do tej funkcji jako zgodnej platformy z .NET Standard, nawet jeśli nie są.</span><span class="sxs-lookup"><span data-stu-id="5be81-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="5be81-170">Chociaż polecenie "Portable-net451 + Win8" nie jest zgodne 100%, jest zgodne .NET Standard ze zbyt dużą ilością dla przejścia z PCL do .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="5be81-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="5be81-171">Importy mogą zostać usunięte, gdy zależności EF ostatecznie uaktualnią do .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="5be81-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="5be81-172">Do "Imports" można dodać wiele struktur w składni tablicy.</span><span class="sxs-lookup"><span data-stu-id="5be81-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="5be81-173">Inne Importy mogą być konieczne w przypadku dodania do projektu dodatkowych bibliotek.</span><span class="sxs-lookup"><span data-stu-id="5be81-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="5be81-174">Zobacz [#5176 problemu](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="5be81-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
