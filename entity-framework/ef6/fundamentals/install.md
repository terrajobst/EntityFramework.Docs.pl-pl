---
title: Pobierz Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419469"
---
# <a name="get-entity-framework"></a><span data-ttu-id="b15fb-102">Pobieranie platformy Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b15fb-102">Get Entity Framework</span></span>
<span data-ttu-id="b15fb-103">Entity Framework składa się z narzędzi EF dla programu Visual Studio i środowiska uruchomieniowego EF.</span><span class="sxs-lookup"><span data-stu-id="b15fb-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="b15fb-104">Narzędzia EF Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b15fb-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="b15fb-105">Entity Framework Tools dla programu Visual Studio zawiera kreatora Dr Designer i dr Model Wizard i są wymagane dla pierwszej bazy danych i modeluje pierwsze przepływy pracy.</span><span class="sxs-lookup"><span data-stu-id="b15fb-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="b15fb-106">Narzędzia Dr są dołączone do wszystkich najnowszych wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b15fb-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="b15fb-107">Jeśli wykonujesz niestandardową instalację programu Visual Studio, musisz upewnić się, że element "Entity Framework 6 Tools" jest wybierany przez wybranie obciążenia, które je zawiera, lub wybranie go jako pojedynczego składnika.</span><span class="sxs-lookup"><span data-stu-id="b15fb-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="b15fb-108">W przypadku niektórych wcześniejszych wersji programu Visual Studio zaktualizowane narzędzia EF są dostępne jako pobrane.</span><span class="sxs-lookup"><span data-stu-id="b15fb-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="b15fb-109">Zobacz [wersje programu Visual Studio](~/ef6/what-is-new/visual-studio.md) , aby uzyskać wskazówki dotyczące uzyskiwania najnowszej wersji narzędzi EF dostępnych dla używanej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b15fb-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="b15fb-110">Środowisko uruchomieniowe EF</span><span class="sxs-lookup"><span data-stu-id="b15fb-110">EF Runtime</span></span>

<span data-ttu-id="b15fb-111">Najnowsza wersja Entity Framework jest dostępna jako [pakiet NuGet EntityFramework](https://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="b15fb-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b15fb-112">Jeśli nie znasz Menedżera pakietów NuGet, zachęcamy do przeczytania [omówienia narzędzia NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="b15fb-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="b15fb-113">Instalowanie pakietu NuGet EF</span><span class="sxs-lookup"><span data-stu-id="b15fb-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="b15fb-114">Aby zainstalować pakiet EntityFramework, kliknij prawym przyciskiem myszy folder **odwołania** projektu i wybierz polecenie **Zarządzaj pakietami NuGet...**</span><span class="sxs-lookup"><span data-stu-id="b15fb-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Zarządzanie pakietami NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="b15fb-116">Instalowanie z konsoli Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="b15fb-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="b15fb-117">Alternatywnie możesz zainstalować EntityFramework, uruchamiając następujące polecenie w [konsoli Menedżera pakietów](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="b15fb-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="b15fb-118">Instalowanie określonej wersji programu EF</span><span class="sxs-lookup"><span data-stu-id="b15fb-118">Installing a specific version of EF</span></span>

<span data-ttu-id="b15fb-119">Z EF 4,1, nowe wersje środowiska uruchomieniowego EF zostały wydane jako [pakiet NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="b15fb-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b15fb-120">Dowolną z tych wersji można dodać do projektu opartego na .NET Framework, uruchamiając następujące polecenie w [konsoli Menedżera pakietów](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b15fb-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="b15fb-121">Należy pamiętać, że `<number>` reprezentuje określoną wersję EF do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="b15fb-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="b15fb-122">Na przykład 6.2.0 jest wersją numeru dla EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="b15fb-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="b15fb-123">Środowiska uruchomieniowe EF przed 4,1 były częścią .NET Framework i nie można ich instalować oddzielnie.</span><span class="sxs-lookup"><span data-stu-id="b15fb-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="b15fb-124">Instalowanie najnowszej wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="b15fb-124">Installing the Latest Preview</span></span>

<span data-ttu-id="b15fb-125">Powyższe metody zapewniają najnowszą w pełni obsługiwaną wersję Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b15fb-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="b15fb-126">Dostępne są często wstępne wersje Entity Framework dostępnych, które można wypróbować i przekazać nam swoją opinię.</span><span class="sxs-lookup"><span data-stu-id="b15fb-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="b15fb-127">Aby zainstalować najnowszą wersję zapoznawczą EntityFramework, możesz wybrać opcję **Uwzględnij wersję wstępną** w oknie Zarządzaj pakietami NuGet.</span><span class="sxs-lookup"><span data-stu-id="b15fb-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="b15fb-128">Jeśli wersje wstępne nie są dostępne, automatycznie otrzymasz najnowszą w pełni obsługiwaną wersję programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b15fb-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Uwzględnij wersję wstępną](~/ef6/media/includeprerelease.png)

<span data-ttu-id="b15fb-130">Alternatywnie możesz uruchomić następujące polecenie w [konsoli Menedżera pakietów](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="b15fb-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
