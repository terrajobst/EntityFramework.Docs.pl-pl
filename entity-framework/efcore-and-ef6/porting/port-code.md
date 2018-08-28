---
title: Przenoszenie z programu EF6 do programu EF Core — przenoszenie modelu opartego na kodzie
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997052"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="70cec-102">Przenoszenie modelu opartego na kodzie EF6 do programu EF Core</span><span class="sxs-lookup"><span data-stu-id="70cec-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="70cec-103">Jeśli wszystkie ostrzeżenia zostały przeczytane, możesz przystąpić do portu, poniżej przedstawiono wskazówki, które pomogą Ci rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="70cec-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="70cec-104">Instalowanie pakietów programu EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="70cec-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="70cec-105">Aby korzystać z programu EF Core, zainstaluj pakiet NuGet dla dostawcy bazy danych, którego chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="70cec-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="70cec-106">Na przykład, gdy obiektem docelowym programu SQL Server, można zainstalować `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="70cec-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="70cec-107">Zobacz [dostawcy baz danych](../../core/providers/index.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="70cec-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="70cec-108">Jeśli planujesz użyć migracje, a następnie należy również zainstalować `Microsoft.EntityFrameworkCore.Tools` pakietu.</span><span class="sxs-lookup"><span data-stu-id="70cec-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="70cec-109">Jest dobrym rozwiązaniem pozostawić pakiet NuGet platformy EF6 (EntityFramework), zainstalowane, zgodnie z programem EF Core i EF6 mogą być używane side-by-side w tej samej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="70cec-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="70cec-110">Jednak w przypadku korzystania z platformy EF6 w żadnych obszarów aplikacji nie są pomyślny przebieg operacji, następnie odinstalowaniu pakietu pomoże spowodować błędy kompilacji na fragmenty kodu, które wymagają uwagi.</span><span class="sxs-lookup"><span data-stu-id="70cec-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="70cec-111">Przestrzenie nazw wymiany</span><span class="sxs-lookup"><span data-stu-id="70cec-111">Swap namespaces</span></span>

<span data-ttu-id="70cec-112">Większość interfejsów API, których używasz w EF6 znajdują się w `System.Data.Entity` przestrzeni nazw (i pokrewnych przestrzeniach nazw sub).</span><span class="sxs-lookup"><span data-stu-id="70cec-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="70cec-113">Pierwszy zmiana kodu jest można zamienić na `Microsoft.EntityFrameworkCore` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="70cec-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="70cec-114">Będzie zazwyczaj rozpocząć pochodnej kontekstu pliku kodu, a następnie pozwolimy na opracowanie stamtąd adresowania błędy kompilacji w miarę ich występowania.</span><span class="sxs-lookup"><span data-stu-id="70cec-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="70cec-115">Konfiguracja kontekstu (połączenie itp.)</span><span class="sxs-lookup"><span data-stu-id="70cec-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="70cec-116">Zgodnie z opisem w [upewnij się, EF Core będzie pracy dla aplikacji](ensure-requirements.md), programem EF Core ma mniej magic wokół wykrywanie bazy danych, aby nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="70cec-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="70cec-117">Należy zastąpić `OnConfiguring` metodę w pochodnej kontekstu i użyj interfejsu API określonego dostawcy bazy danych, aby skonfigurować połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="70cec-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="70cec-118">Większość aplikacji EF6 przechowywanie parametrów połączenia w aplikacjach `App/Web.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="70cec-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="70cec-119">W programie EF Core przeczytanie tego parametry połączenia za pomocą `ConfigurationManager` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="70cec-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="70cec-120">Może być konieczne dodanie odwołania do `System.Configuration` zestawu struktury, aby można było używać tego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="70cec-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="70cec-121">Aktualizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="70cec-121">Update your code</span></span>

<span data-ttu-id="70cec-122">W tym momencie jest kwestią adresowania błędy kompilacji i przeglądania kodu, aby zobaczyć zmiany zachowania będzie miało wpływ użytkownik.</span><span class="sxs-lookup"><span data-stu-id="70cec-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="70cec-123">Migracja istniejących</span><span class="sxs-lookup"><span data-stu-id="70cec-123">Existing migrations</span></span>

<span data-ttu-id="70cec-124">Tak naprawdę nie jest to możliwe sposobu portu istniejących migracje platformy EF6 do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="70cec-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="70cec-125">Jeśli to możliwe najlepiej Załóżmy, że zostały zastosowane wszystkie poprzednich migracji z programu EF6 do bazy danych, a następnie rozpoczęcia migracji schematu od tego punktu, przy użyciu programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="70cec-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="70cec-126">Aby to zrobić, należy użyć `Add-Migration` polecenie do dodania do migracji, gdy model jest przenoszone do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="70cec-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="70cec-127">Następnie należy usunąć cały kod z `Up` i `Down` metody szkieletu migracji.</span><span class="sxs-lookup"><span data-stu-id="70cec-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="70cec-128">Porównuje następnej migracji do modelu podczas tej początkowej migracji został szkielet.</span><span class="sxs-lookup"><span data-stu-id="70cec-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="70cec-129">Testowanie portu</span><span class="sxs-lookup"><span data-stu-id="70cec-129">Test the port</span></span>

<span data-ttu-id="70cec-130">Po prostu, ponieważ kompiluje aplikację, nie znaczy, że pomyślnie są przenoszone do programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="70cec-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="70cec-131">Należy przetestować wszystkie obszary w aplikacji, aby upewnić się, że żadne zmiany zachowania niekorzystnie wpłynąć na nie wpływ aplikacji.</span><span class="sxs-lookup"><span data-stu-id="70cec-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
