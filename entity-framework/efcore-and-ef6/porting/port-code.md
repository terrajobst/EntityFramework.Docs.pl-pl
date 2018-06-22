---
title: Eksportowanie z EF6 do EF Core - eksportowanie Model oparty na kod
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054284"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="d3d10-102">Eksportowanie EF6 opartej na kodzie modelu EF Core</span><span class="sxs-lookup"><span data-stu-id="d3d10-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="d3d10-103">Jeśli wszystkie ostrzeżenia zostały przeczytane i wszystko jest gotowe do portu, poniżej przedstawiono wskazówki, które pomogą Ci rozpocząć.</span><span class="sxs-lookup"><span data-stu-id="d3d10-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="d3d10-104">Instalowanie pakietów EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="d3d10-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="d3d10-105">Aby używać EF Core, należy zainstalować pakiet NuGet dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="d3d10-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="d3d10-106">Na przykład gdy program SQL Server, czy zainstalować `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="d3d10-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="d3d10-107">Zobacz [dostawcy bazy danych](../../core/providers/index.md) szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="d3d10-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="d3d10-108">Jeśli planujesz użyć migracji, należy również zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="d3d10-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="d3d10-109">Bardzo można pozostawić pakiet EF6 NuGet (EntityFramework), zainstalowane, EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d3d10-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="d3d10-110">Jednak jeśli nie są zamierza użyć EF6 w obszary aplikacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="d3d10-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="d3d10-111">Zamień przestrzenie nazw</span><span class="sxs-lookup"><span data-stu-id="d3d10-111">Swap namespaces</span></span>

<span data-ttu-id="d3d10-112">Większość interfejsów API, które są używane w EF6 znajdują się w `System.Data.Entity` przestrzeni nazw (i związane z przestrzeni nazw sub).</span><span class="sxs-lookup"><span data-stu-id="d3d10-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="d3d10-113">Pierwsza zmiana kodu jest można zamienić na `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="d3d10-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="d3d10-114">Czy zazwyczaj rozpoczyna się z plikiem pochodnej kontekstu kodu i następnie wyglądają stamtąd adresowania błędy kompilacji w miarę ich występowania.</span><span class="sxs-lookup"><span data-stu-id="d3d10-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="d3d10-115">Kontekst konfiguracji (połączenie itp.)</span><span class="sxs-lookup"><span data-stu-id="d3d10-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="d3d10-116">Zgodnie z opisem w [upewnij się, EF rdzenie będą pracy Twoja aplikacja](ensure-requirements.md), EF Core ma mniej magic wokół wykrywanie bazy danych, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="d3d10-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="d3d10-117">Należy zastąpić `OnConfiguring` metodę w pochodnej kontekstu i nawiązanie połączenia z bazą danych, użyj interfejsu API określonego dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3d10-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="d3d10-118">Większość aplikacji EF6 przechowywanie parametrów połączenia w aplikacjach `App/Web.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="d3d10-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="d3d10-119">W podstawowej EF przeczytanie tego parametry połączenia za pomocą `ConfigurationManager` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d3d10-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="d3d10-120">Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="d3d10-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="d3d10-121">Zaktualizuj kod</span><span class="sxs-lookup"><span data-stu-id="d3d10-121">Update your code</span></span>

<span data-ttu-id="d3d10-122">W tym momencie jest kwestią adresowania błędy kompilacji i przeglądania kodu w celu zmiany zachowania będzie miało wpływ możesz się w temacie.</span><span class="sxs-lookup"><span data-stu-id="d3d10-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="d3d10-123">Migracja istniejących</span><span class="sxs-lookup"><span data-stu-id="d3d10-123">Existing migrations</span></span>

<span data-ttu-id="d3d10-124">Naprawdę jest możliwe sposobem portu istniejących migracje EF6 EF Core.</span><span class="sxs-lookup"><span data-stu-id="d3d10-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="d3d10-125">Jeśli to możliwe najlepiej założono, że wszystkie poprzednich migracji z EF6 zostały zastosowane do bazy danych i następnie rozpoczęcia migracji schematu od tego punktu, przy użyciu EF Core.</span><span class="sxs-lookup"><span data-stu-id="d3d10-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="d3d10-126">Aby to zrobić, należy użyć `Add-Migration` polecenie, aby dodać migracji, gdy model jest przenoszone do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d3d10-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="d3d10-127">Następnie spowoduje usunięcie całego kodu ze `Up` i `Down` metody szkieletu migracji.</span><span class="sxs-lookup"><span data-stu-id="d3d10-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="d3d10-128">Kolejne migracje zostanie porównany do modelu po tej migracji początkowe Tworzenie szkieletu wykonano.</span><span class="sxs-lookup"><span data-stu-id="d3d10-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="d3d10-129">Testowanie portu</span><span class="sxs-lookup"><span data-stu-id="d3d10-129">Test the port</span></span>

<span data-ttu-id="d3d10-130">Tak, ponieważ aplikacja kompiluje, oznacza to, że pomyślnie jest przenoszone na EF rdzeń.</span><span class="sxs-lookup"><span data-stu-id="d3d10-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="d3d10-131">Należy przetestować wszystkie obszary w aplikacji w celu zapewnienia, że zmiany zachowania ma negatywny wpływ na aplikację.</span><span class="sxs-lookup"><span data-stu-id="d3d10-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
