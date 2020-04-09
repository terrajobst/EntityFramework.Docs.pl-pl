---
title: Przenoszenie z EF6 do EF Core — przenoszenie modelu opartego na kodzie — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419638"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="db8eb-102">Przenoszenie modelu opartego na kodzie EF6 do ef core</span><span class="sxs-lookup"><span data-stu-id="db8eb-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="db8eb-103">Jeśli przeczytałeś wszystkie zastrzeżenia i jesteś gotowy do portu, oto kilka wskazówek, które pomogą Ci rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="db8eb-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="db8eb-104">Instalowanie pakietów EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="db8eb-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="db8eb-105">Aby użyć EF Core, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="db8eb-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="db8eb-106">Na przykład podczas kierowania na program `Microsoft.EntityFrameworkCore.SqlServer`SQL Server należy zainstalować program .</span><span class="sxs-lookup"><span data-stu-id="db8eb-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="db8eb-107">Zobacz [dostawców baz danych, aby](../../core/providers/index.md) uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="db8eb-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="db8eb-108">Jeśli planujesz używać migracji, należy również zainstalować `Microsoft.EntityFrameworkCore.Tools` pakiet.</span><span class="sxs-lookup"><span data-stu-id="db8eb-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="db8eb-109">Jest w porządku, aby pozostawić ef6 NuGet pakiet (EntityFramework) zainstalowany, jak EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8eb-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="db8eb-110">Jednak jeśli nie zamierzasz używać EF6 w żadnych obszarach aplikacji, a następnie odinstalowanie pakietu pomoże dać błędy kompilacji na kawałki kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="db8eb-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="db8eb-111">Zamienianie obszarów nazw</span><span class="sxs-lookup"><span data-stu-id="db8eb-111">Swap namespaces</span></span>

<span data-ttu-id="db8eb-112">Większość interfejsów API używanych w u `System.Data.Entity` ef6 znajdują się w obszarze nazw (i powiązanych pod-obszarów nazw).</span><span class="sxs-lookup"><span data-stu-id="db8eb-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="db8eb-113">Pierwszą zmianą kodu jest `Microsoft.EntityFrameworkCore` zamiana na obszar nazw.</span><span class="sxs-lookup"><span data-stu-id="db8eb-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="db8eb-114">Zazwyczaj należy rozpocząć od pliku kodu kontekstu pochodnego, a następnie wypracować stamtąd, adresowanie błędów kompilacji, jak występują.</span><span class="sxs-lookup"><span data-stu-id="db8eb-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="db8eb-115">Konfiguracja kontekstu (połączenie itp.)</span><span class="sxs-lookup"><span data-stu-id="db8eb-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="db8eb-116">Zgodnie z opisem w [Ensure EF Core will work for Your Application](ensure-requirements.md), EF Core ma mniej magii wokół wykrywania bazy danych, aby połączyć się z.</span><span class="sxs-lookup"><span data-stu-id="db8eb-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="db8eb-117">Należy zastąpić `OnConfiguring` metodę w kontekście pochodnym i użyć interfejsu API specyficznego dla dostawcy bazy danych do skonfigurowania połączenia z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="db8eb-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="db8eb-118">Większość aplikacji EF6 przechowuje parametry `App/Web.config` połączenia w pliku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="db8eb-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="db8eb-119">W EF Core odczytasz ten `ConfigurationManager` ciąg połączenia przy użyciu interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="db8eb-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="db8eb-120">Może być konieczne dodanie odwołania `System.Configuration` do zestawu framework, aby móc korzystać z tego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="db8eb-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="db8eb-121">Aktualizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="db8eb-121">Update your code</span></span>

<span data-ttu-id="db8eb-122">W tym momencie jest to kwestia adresowania błędów kompilacji i przeglądania kodu, aby sprawdzić, czy zmiany zachowania będą miały wpływ na Ciebie.</span><span class="sxs-lookup"><span data-stu-id="db8eb-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="db8eb-123">Istniejące migracje</span><span class="sxs-lookup"><span data-stu-id="db8eb-123">Existing migrations</span></span>

<span data-ttu-id="db8eb-124">Nie ma naprawdę wykonalny sposób na przeniesienie istniejących migracji EF6 do EF Core.</span><span class="sxs-lookup"><span data-stu-id="db8eb-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="db8eb-125">Jeśli to możliwe, najlepiej jest założyć, że wszystkie poprzednie migracje z EF6 zostały zastosowane do bazy danych, a następnie rozpocząć migrację schematu z tego punktu przy użyciu EF Core.</span><span class="sxs-lookup"><span data-stu-id="db8eb-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="db8eb-126">Aby to zrobić, należy `Add-Migration` użyć polecenia, aby dodać migracji, gdy model jest przeniesiony do EF Core.</span><span class="sxs-lookup"><span data-stu-id="db8eb-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="db8eb-127">Następnie należy usunąć cały `Up` kod `Down` z i metody migracji szkieletu.</span><span class="sxs-lookup"><span data-stu-id="db8eb-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="db8eb-128">Kolejne migracje zostaną porównane z modelem, gdy ta początkowa migracja została szkieletowa.</span><span class="sxs-lookup"><span data-stu-id="db8eb-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="db8eb-129">Przetestuj port</span><span class="sxs-lookup"><span data-stu-id="db8eb-129">Test the port</span></span>

<span data-ttu-id="db8eb-130">Tylko dlatego, że aplikacja kompiluje, nie oznacza, że jest pomyślnie przeniesiony do EF Core.</span><span class="sxs-lookup"><span data-stu-id="db8eb-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="db8eb-131">Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna ze zmian zachowania nie wpłynęła negatywnie na aplikację.</span><span class="sxs-lookup"><span data-stu-id="db8eb-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
