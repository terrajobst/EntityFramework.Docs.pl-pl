---
title: Obsługa dostawcy dla typów przestrzennych — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416340"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="b7bc1-102">Obsługa dostawcy dla typów przestrzennych</span><span class="sxs-lookup"><span data-stu-id="b7bc1-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="b7bc1-103">Entity Framework obsługuje pracę z danymi przestrzennymi za pomocą klas DbGeography lub DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="b7bc1-104">Te klasy są oparte na funkcjach specyficznych dla bazy danych oferowanych przez dostawcę Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="b7bc1-105">Nie wszyscy dostawcy obsługują dane przestrzenne, a te, które mogą mieć dodatkowe wymagania wstępne, takie jak instalacja zestawów typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="b7bc1-106">Więcej informacji o obsłudze dostawcy dla typów przestrzennych znajduje się poniżej.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="b7bc1-107">Dodatkowe informacje na temat korzystania z typów przestrzennych w aplikacji można znaleźć w dwóch przewodnikach, jeden dla Code First, drugi dla Database First lub Model First:</span><span class="sxs-lookup"><span data-stu-id="b7bc1-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="b7bc1-108">Typy danych przestrzennych w Code First</span><span class="sxs-lookup"><span data-stu-id="b7bc1-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="b7bc1-109">Typy danych przestrzennych w projektancie EF</span><span class="sxs-lookup"><span data-stu-id="b7bc1-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="b7bc1-110">Wersje EF obsługujące typy przestrzenne</span><span class="sxs-lookup"><span data-stu-id="b7bc1-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="b7bc1-111">Obsługa typów przestrzennych została wprowadzona w EF5.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="b7bc1-112">Jednak w przypadku typów przestrzennych EF5 są obsługiwane tylko wtedy, gdy aplikacja jest uruchamiana i działa na platformie .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="b7bc1-113">Począwszy od typów przestrzennych EF6 są obsługiwane w przypadku aplikacji przeznaczonych dla programów .NET 4 i .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="b7bc1-114">Dostawcy EF, którzy obsługują typy przestrzenne</span><span class="sxs-lookup"><span data-stu-id="b7bc1-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="b7bc1-115">EF5</span><span class="sxs-lookup"><span data-stu-id="b7bc1-115">EF5</span></span>  

<span data-ttu-id="b7bc1-116">Entity Framework dostawców dla EF5, że mamy świadomość, że typy przestrzenne są następujące:</span><span class="sxs-lookup"><span data-stu-id="b7bc1-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="b7bc1-117">Dostawca Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b7bc1-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="b7bc1-118">Ten dostawca jest dostarczany jako część EF5.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="b7bc1-119">Ten dostawca zależy od pewnych dodatkowych bibliotek niskiego poziomu, które mogą wymagać zainstalowania — Zobacz poniżej, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="b7bc1-120">Devart dotConnect dla programu Oracle</span><span class="sxs-lookup"><span data-stu-id="b7bc1-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="b7bc1-121">Jest to dostawca innych firm od Devart.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="b7bc1-122">Jeśli znasz dostawcę EF5, który obsługuje typy przestrzenne, skontaktuj się z nami i będziemy mogli dodać go do tej listy.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="b7bc1-123">EF6</span><span class="sxs-lookup"><span data-stu-id="b7bc1-123">EF6</span></span>  

<span data-ttu-id="b7bc1-124">Entity Framework dostawców dla EF6, że mamy świadomość, że typy przestrzenne są następujące:</span><span class="sxs-lookup"><span data-stu-id="b7bc1-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="b7bc1-125">Dostawca Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b7bc1-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="b7bc1-126">Ten dostawca jest dostarczany jako część EF6.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="b7bc1-127">Ten dostawca zależy od pewnych dodatkowych bibliotek niskiego poziomu, które mogą wymagać zainstalowania — Zobacz poniżej, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="b7bc1-128">Devart dotConnect dla programu Oracle</span><span class="sxs-lookup"><span data-stu-id="b7bc1-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="b7bc1-129">Jest to dostawca innych firm od Devart.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="b7bc1-130">Jeśli znasz dostawcę EF6, który obsługuje typy przestrzenne, skontaktuj się z nami i będziemy mogli dodać go do tej listy.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="b7bc1-131">Wymagania wstępne dotyczące typów przestrzennych z Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b7bc1-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="b7bc1-132">Obsługa SQL Server przestrzennej zależy od typów SqlGeography i SqlGeometry o niskim poziomie SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="b7bc1-133">Te typy na żywo w zestawie Microsoft. SqlServer. Types. dll i ten zestaw nie jest dostarczany jako część EF lub jako część .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="b7bc1-134">Gdy program Visual Studio jest zainstalowany, często instaluje również wersję SQL Server i obejmuje instalację Microsoft. SqlServer. Types. dll.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="b7bc1-135">Jeśli SQL Server nie jest zainstalowana na komputerze, na którym mają być używane typy przestrzenne, lub jeśli typy przestrzenne zostały wykluczone z instalacji SQL Server, należy zainstalować je ręcznie.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="b7bc1-136">Typy można instalować przy użyciu `SQLSysClrTypes.msi`, który jest częścią Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="b7bc1-137">Typy przestrzenne są SQL Server specyficzne dla wersji, dlatego zalecamy [przeszukanie "SQL Server pakiet Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) w centrum pobierania Microsoft, a następnie wybranie i pobranie opcji odpowiadającej używanej wersji SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b7bc1-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
