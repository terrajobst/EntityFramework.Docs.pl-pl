---
title: Przenoszenie z EF6 do EF Core-Porting model-EF oparty na kodzie
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181216"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="df070-102">Przenoszenie modelu opartego na kodzie EF6 do EF Core</span><span class="sxs-lookup"><span data-stu-id="df070-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="df070-103">Jeśli przeczytasz wszystkie zastrzeżenia i wszystko jest gotowe do portu, Oto kilka wytycznych, które ułatwią Ci rozpoczęcie pracy.</span><span class="sxs-lookup"><span data-stu-id="df070-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="df070-104">Zainstaluj EF Core pakiety NuGet</span><span class="sxs-lookup"><span data-stu-id="df070-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="df070-105">Aby użyć EF Core, należy zainstalować pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="df070-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="df070-106">Na przykład podczas określania wartości docelowej SQL Server należy zainstalować `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="df070-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="df070-107">Aby uzyskać szczegółowe informacje, zobacz [dostawcy bazy danych](../../core/providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="df070-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="df070-108">Jeśli planujesz używać migracji, należy również zainstalować pakiet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="df070-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="df070-109">Warto pozostawić zainstalowany pakiet NuGet EF6 (EntityFramework), ponieważ EF Core i EF6 mogą być używane obok siebie w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="df070-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="df070-110">Jeśli jednak nie zamierzasz korzystać z EF6 w żadnych obszarach aplikacji, dezinstalacja pakietu pomoże Ci w wydaniu błędów kompilacji dla fragmentów kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="df070-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="df070-111">Zamień przestrzenie nazw</span><span class="sxs-lookup"><span data-stu-id="df070-111">Swap namespaces</span></span>

<span data-ttu-id="df070-112">Większość interfejsów API, które są używane w EF6, znajduje się w przestrzeni nazw `System.Data.Entity` (wraz z powiązanymi przestrzeniami nazw).</span><span class="sxs-lookup"><span data-stu-id="df070-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="df070-113">Pierwsza zmiana kodu polega na zamianie na przestrzeń nazw `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="df070-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="df070-114">Zwykle zaczynasz od pochodnego pliku kodu kontekstu, a następnie z tego powodu wykorzystasz błędy kompilacji w miarę ich występowania.</span><span class="sxs-lookup"><span data-stu-id="df070-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="df070-115">Konfiguracja kontekstu (połączenie itp.)</span><span class="sxs-lookup"><span data-stu-id="df070-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="df070-116">Zgodnie z opisem w temacie upewnij się, że [EF Core będzie działała dla aplikacji](ensure-requirements.md), EF Core ma mniej Magic wokół wykrywania bazy danych w celu nawiązania połączenia.</span><span class="sxs-lookup"><span data-stu-id="df070-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="df070-117">Należy zastąpić metodę `OnConfiguring` w kontekście pochodnym i skonfigurować połączenie z bazą danych przy użyciu interfejsu API specyficznego dla dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="df070-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="df070-118">Większość aplikacji EF6 przechowuje parametry połączenia w plikach aplikacji `App/Web.config`.</span><span class="sxs-lookup"><span data-stu-id="df070-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="df070-119">W EF Core należy odczytać te parametry połączenia za pomocą interfejsu API `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="df070-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="df070-120">Może być konieczne dodanie odwołania do zestawu platformy `System.Configuration`, aby można było korzystać z tego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="df070-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="df070-121">Aktualizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="df070-121">Update your code</span></span>

<span data-ttu-id="df070-122">W tym momencie jest kwestią rozwiązywania błędów kompilacji i przeglądania kodu, aby sprawdzić, czy zmiany zachowania wpłyną na Ciebie.</span><span class="sxs-lookup"><span data-stu-id="df070-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="df070-123">Istniejące migracje</span><span class="sxs-lookup"><span data-stu-id="df070-123">Existing migrations</span></span>

<span data-ttu-id="df070-124">Nie istnieje naprawdę możliwy do przenoszenia istniejących EF6 migracji do EF Core.</span><span class="sxs-lookup"><span data-stu-id="df070-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="df070-125">Jeśli to możliwe, najlepiej założyć, że wszystkie poprzednie migracje z EF6 zostały zastosowane do bazy danych, a następnie rozpocznij migrację schematu z tego punktu przy użyciu EF Core.</span><span class="sxs-lookup"><span data-stu-id="df070-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="df070-126">W tym celu należy użyć polecenia `Add-Migration`, aby dodać migrację po przeprowadzeniu modelu do EF Core.</span><span class="sxs-lookup"><span data-stu-id="df070-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="df070-127">Następnie można usunąć cały kod z metod `Up` i `Down` migracji szkieletowej.</span><span class="sxs-lookup"><span data-stu-id="df070-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="df070-128">Kolejne migracje zostaną porównane z modelem, gdy początkowa migracja została poddana migracji.</span><span class="sxs-lookup"><span data-stu-id="df070-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="df070-129">Testowanie portu</span><span class="sxs-lookup"><span data-stu-id="df070-129">Test the port</span></span>

<span data-ttu-id="df070-130">Tak samo, jak Twoja aplikacja kompiluje, nie oznacza, że została pomyślnie przeniesiono do EF Core.</span><span class="sxs-lookup"><span data-stu-id="df070-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="df070-131">Należy przetestować wszystkie obszary aplikacji, aby upewnić się, że żadna zmiana zachowania nie miała negatywnego wpływu na aplikację.</span><span class="sxs-lookup"><span data-stu-id="df070-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
