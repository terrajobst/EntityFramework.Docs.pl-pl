---
title: Pobieranie programu Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 78ef1e7b20bd879c972870552c8f692e153b7abb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996568"
---
# <a name="get-entity-framework"></a><span data-ttu-id="5f077-102">Pobieranie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="5f077-102">Get Entity Framework</span></span>
<span data-ttu-id="5f077-103">Entity Framework składa się z narzędziami EF dla programu Visual Studio i środowiska uruchomieniowego EF.</span><span class="sxs-lookup"><span data-stu-id="5f077-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="5f077-104">EF Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f077-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="5f077-105">Narzędzi Entity Framework Tools for Visual Studio obejmują w kreatorze modelu platformy EF i projektancie platformy EF są wymagane dla bazy danych po raz pierwszy i modelowanie pierwszy przepływów pracy.</span><span class="sxs-lookup"><span data-stu-id="5f077-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="5f077-106">Narzędzia platformy EF znajdują się w najnowszych wersjach programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f077-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="5f077-107">Jeśli wykonać niestandardową instalację programu Visual Studio, musisz upewnić się, że element "Entity Framework 6 Tools" jest zaznaczone, wybierając obciążenia zawierający go lub wybierając go jako poszczególnych składników.</span><span class="sxs-lookup"><span data-stu-id="5f077-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="5f077-108">W przypadku niektórych poprzednich wersji programu Visual Studio zaktualizowanych narzędzi EF są dostępne do pobrania.</span><span class="sxs-lookup"><span data-stu-id="5f077-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="5f077-109">Zobacz [wersje serwera Visual Studio](~/ef6/what-is-new/visual-studio.md) wskazówki dotyczące sposobu uzyskania najnowszej wersji programu EF narzędzi dostępnych dla używanej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f077-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="5f077-110">Środowiska uruchomieniowego EF</span><span class="sxs-lookup"><span data-stu-id="5f077-110">EF Runtime</span></span>

<span data-ttu-id="5f077-111">Najnowszą wersję programu Entity Framework jest dostępna jako [pakiet NuGet platformy EntityFramework](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="5f077-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="5f077-112">Jeśli użytkownik nie jest zaznajomiony z Menedżera pakietów NuGet, zachęcamy do odczytania [Przegląd NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="5f077-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="5f077-113">Instalowanie pakietu NuGet programu EF</span><span class="sxs-lookup"><span data-stu-id="5f077-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="5f077-114">Można zainstalować pakiet platformy EntityFramework, klikając prawym przyciskiem myszy **odwołania** folderu projektu i wybierając polecenie **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="5f077-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="5f077-116">Instalowanie za pomocą konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="5f077-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="5f077-117">Alternatywnie, można zainstalować platformy EntityFramework, uruchamiając następujące polecenie w [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="5f077-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="5f077-118">Instalowanie określonej wersji programu EF</span><span class="sxs-lookup"><span data-stu-id="5f077-118">Installing a specific version of EF</span></span>

<span data-ttu-id="5f077-119">Od wersji 4.1 EF wydano nowe wersje środowiska uruchomieniowego EF jako [EntityFramework NuGet Pacakge](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="5f077-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Pacakge](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="5f077-120">Dowolne z tych wersji można dodać do projektu na podstawie .NET Framework, uruchamiając następujące polecenie w programie Visual Studio [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="5f077-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="5f077-121">Należy pamiętać, że `<number>` reprezentuje określoną wersję platformy EF do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="5f077-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="5f077-122">Na przykład 6.2.0 stanowi wersję numer EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="5f077-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="5f077-123">EF środowisk wykonawczych, zanim 4.1 były częścią środowiska .NET Framework i nie można zainstalować oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="5f077-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="5f077-124">Instalowanie najnowszej wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="5f077-124">Installing the Latest Preview</span></span>

<span data-ttu-id="5f077-125">Powyższych metod prześle Ci najnowsze wydania Entity Framework w pełni obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="5f077-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="5f077-126">Często są wersje wstępne programu Entity Framework jest dostępna, że chętnie można wypróbować, a następnie Prześlij nam opinię na temat.</span><span class="sxs-lookup"><span data-stu-id="5f077-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="5f077-127">Do zainstalowania najnowszej wersji zapoznawczej platformy EntityFramework, możesz wybrać **Uwzględnij wydania wstępne** w oknie Zarządzanie pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="5f077-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="5f077-128">Jeśli nie wstępnej wersji są dostępne najnowsze zostanie automatycznie pobrana w pełni obsługiwana wersja programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5f077-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![IncludePreRelease](~/ef6/media/includeprerelease.png)

<span data-ttu-id="5f077-130">Alternatywnie, możesz uruchomić następujące polecenie w [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="5f077-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
