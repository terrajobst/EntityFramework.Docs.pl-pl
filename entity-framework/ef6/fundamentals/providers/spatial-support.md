---
title: Obsługa dostawców dla typów przestrzennych - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022278"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="80e89-102">Obsługa dostawców dla typów przestrzennych</span><span class="sxs-lookup"><span data-stu-id="80e89-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="80e89-103">Entity Framework obsługuje pracę z danymi przestrzennymi za pośrednictwem DbGeography lub DbGeometry klasy.</span><span class="sxs-lookup"><span data-stu-id="80e89-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="80e89-104">W ramach tych zajęć, zależą od funkcji specyficznych dla bazy danych udostępnianymi przez dostawcę programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="80e89-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="80e89-105">Nie wszyscy dostawcy obsługuje dane przestrzenne i tych, które wykonują może mieć dodatkowe wymagania wstępne, takich jak instalacja zestawów typów przestrzennych.</span><span class="sxs-lookup"><span data-stu-id="80e89-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="80e89-106">Więcej informacji na temat Obsługa dostawców dla typów przestrzennych znajduje się poniżej.</span><span class="sxs-lookup"><span data-stu-id="80e89-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="80e89-107">Dodatkowe informacje dotyczące sposobu używania typów przestrzennych w aplikacji znajdują się w dwóch wskazówki, jeden dla Code First, drugie dla pierwszej bazy danych lub pierwszego modelu:</span><span class="sxs-lookup"><span data-stu-id="80e89-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="80e89-108">Typy w kodzie najpierw danych przestrzennych</span><span class="sxs-lookup"><span data-stu-id="80e89-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="80e89-109">Typy danych przestrzennych w Projektancie platformy EF</span><span class="sxs-lookup"><span data-stu-id="80e89-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="80e89-110">Wersje programu EF, które obsługują typów przestrzennych</span><span class="sxs-lookup"><span data-stu-id="80e89-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="80e89-111">Obsługa typów przestrzennych została wprowadzona w EF5.</span><span class="sxs-lookup"><span data-stu-id="80e89-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="80e89-112">Jednak w EF5 typów przestrzennych są obsługiwane tylko w przypadku aplikacji jest przeznaczony dla i działa w .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="80e89-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="80e89-113">Począwszy od platformy EF6 typów przestrzennych są obsługiwane w przypadku aplikacji przeznaczonych dla platformy .NET 4.5 i .NET 4.</span><span class="sxs-lookup"><span data-stu-id="80e89-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="80e89-114">EF dostawcy, obsługujące typów przestrzennych</span><span class="sxs-lookup"><span data-stu-id="80e89-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="80e89-115">EF5</span><span class="sxs-lookup"><span data-stu-id="80e89-115">EF5</span></span>  

<span data-ttu-id="80e89-116">Dostawcy programu Entity Framework do EF5, które sobie sprawę z czy typów przestrzennych pomocy technicznej:</span><span class="sxs-lookup"><span data-stu-id="80e89-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="80e89-117">Dostawca programu Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="80e89-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="80e89-118">Ten dostawca jest dostarczany jako część EF5.</span><span class="sxs-lookup"><span data-stu-id="80e89-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="80e89-119">Ten dostawca jest zależna od niektórych dodatkowych bibliotek niskiego poziomu, które muszą zostać zainstalowane — Zobacz szczegóły poniżej.</span><span class="sxs-lookup"><span data-stu-id="80e89-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="80e89-120">Devart dotConnect na oprogramowanie Oracle</span><span class="sxs-lookup"><span data-stu-id="80e89-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="80e89-121">Jest to dostawca innych firm z Devart.</span><span class="sxs-lookup"><span data-stu-id="80e89-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="80e89-122">Jeśli znasz EF5 dostawcy obsługująca typów przestrzennych następnie można uzyskać w kontakcie i będziemy wszystkiego dodać go do tej listy.</span><span class="sxs-lookup"><span data-stu-id="80e89-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="80e89-123">EF6</span><span class="sxs-lookup"><span data-stu-id="80e89-123">EF6</span></span>  

<span data-ttu-id="80e89-124">Dostawcy programu Entity Framework, dla platformy EF6, które sobie sprawę z czy typów przestrzennych pomocy technicznej:</span><span class="sxs-lookup"><span data-stu-id="80e89-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="80e89-125">Dostawca programu Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="80e89-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="80e89-126">Ten dostawca jest dostarczany jako część platformy EF6.</span><span class="sxs-lookup"><span data-stu-id="80e89-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="80e89-127">Ten dostawca jest zależna od niektórych dodatkowych bibliotek niskiego poziomu, które muszą zostać zainstalowane — Zobacz szczegóły poniżej.</span><span class="sxs-lookup"><span data-stu-id="80e89-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="80e89-128">Devart dotConnect na oprogramowanie Oracle</span><span class="sxs-lookup"><span data-stu-id="80e89-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="80e89-129">Jest to dostawca innych firm z Devart.</span><span class="sxs-lookup"><span data-stu-id="80e89-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="80e89-130">Jeśli znasz EF6 dostawcy obsługująca typów przestrzennych następnie można uzyskać w kontakcie i będziemy wszystkiego dodać go do tej listy.</span><span class="sxs-lookup"><span data-stu-id="80e89-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="80e89-131">Wymagania wstępne dotyczące typów przestrzennych z programem Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="80e89-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="80e89-132">Obsługa przestrzenne programu SQL Server jest zależna od niskiego poziomu, specyficzne dla programu SQL Server typu SqlGeography i SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="80e89-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="80e89-133">Te typy na żywo w zestawie Microsoft.SqlServer.Types.dll i ten zestaw nie jest dostarczany jako część EF lub w ramach programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="80e89-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="80e89-134">Po zainstalowaniu programu Visual Studio zostanie często również zainstalowana wersja programu SQL Server i będzie to obejmowało instalacji Microsoft.SqlServer.Types.dll.</span><span class="sxs-lookup"><span data-stu-id="80e89-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="80e89-135">Jeśli program SQL Server nie jest zainstalowany na komputerze, na których chcesz użyć typów przestrzennych lub typów przestrzennych zostały wykluczone z instalacji programu SQL Server, należy je zainstalować ręcznie.</span><span class="sxs-lookup"><span data-stu-id="80e89-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="80e89-136">Typy można zainstalować przy użyciu `SQLSysClrTypes.msi`, który jest częścią programu Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="80e89-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="80e89-137">Typów przestrzennych programu SQL Server specyficzny dla wersji są, tak więc zaleca się [Wyszukaj "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) w programie Microsoft Download Center, a następnie wybierz i Pobierz opcji, która odnosi się do wersji programu SQL Server, które będą używane.</span><span class="sxs-lookup"><span data-stu-id="80e89-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
