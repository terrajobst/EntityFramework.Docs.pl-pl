---
title: Co nowego — EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: 9daae787d0cec0ca536413e6263bb363ba76ff2c
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812135"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="8ac87-102">Co nowego w programie EF6</span><span class="sxs-lookup"><span data-stu-id="8ac87-102">What's new in EF6</span></span>

<span data-ttu-id="8ac87-103">Zdecydowanie zalecamy użycie najnowszej wydanej wersji Entity Framework, aby uzyskać najnowsze funkcje i największą stabilność.</span><span class="sxs-lookup"><span data-stu-id="8ac87-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="8ac87-104">Jednak firma Microsoft zdaje sobie sprawę, że może być konieczne użycie poprzedniej wersji lub przeprowadzenie eksperymentu z nowymi ulepszeniami w najnowszej wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="8ac87-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="8ac87-105">Aby zainstalować określone wersje EF, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="8ac87-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="8ac87-106">DR 6.3.0</span><span class="sxs-lookup"><span data-stu-id="8ac87-106">EF 6.3.0</span></span>

<span data-ttu-id="8ac87-107">Środowisko uruchomieniowe EF 6.3.0 zostało wydane dla programu NuGet we wrześniu 2019.</span><span class="sxs-lookup"><span data-stu-id="8ac87-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="8ac87-108">Głównym celem tej wersji jest ułatwienie migracji istniejących aplikacji korzystających z programu EF 6 do platformy .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="8ac87-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="8ac87-109">Społeczność przyczyniła również kilka poprawek i ulepszeń błędów.</span><span class="sxs-lookup"><span data-stu-id="8ac87-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="8ac87-110">Aby uzyskać szczegółowe informacje, zobacz problemy zamknięte w każdym 6.3.0 [punktów kontrolnych](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="8ac87-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="8ac87-111">Poniżej przedstawiono niektóre z bardziej istotnych:</span><span class="sxs-lookup"><span data-stu-id="8ac87-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="8ac87-112">Obsługa platformy .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="8ac87-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="8ac87-113">Pakiet EntityFramework teraz jest przeznaczony dla .NET Standard 2,1 oprócz .NET Framework 4. x.</span><span class="sxs-lookup"><span data-stu-id="8ac87-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="8ac87-114">Oznacza to, że EF 6,3 jest dla wielu platform i jest obsługiwany przez inne systemy operacyjne niż Windows, takie jak Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="8ac87-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="8ac87-115">Polecenia migracji zostały wprowadzone ponownie w celu wykonania poza procesem i pracują z projektami w stylu zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="8ac87-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="8ac87-116">Obsługa SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="8ac87-116">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="8ac87-117">Ulepszona zgodność z Roslyn i NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="8ac87-117">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="8ac87-118">Dodano narzędzie `ef6.exe` do włączania, dodawania, tworzenia skryptów i stosowania migracji z zestawów.</span><span class="sxs-lookup"><span data-stu-id="8ac87-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="8ac87-119">Spowoduje to zastąpienie `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="8ac87-119">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="8ac87-120">Obsługa projektanta EF</span><span class="sxs-lookup"><span data-stu-id="8ac87-120">EF designer support</span></span>

<span data-ttu-id="8ac87-121">Nie jest obecnie obsługiwane używanie projektanta EF bezpośrednio w projektach .NET Core lub .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8ac87-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="8ac87-122">To ograniczenie można obejść przez dodanie pliku EDMX i wygenerowanych klas dla jednostek i DbContext jako połączonych plików do projektu .NET Core 3,0 lub .NET Standard 2,1 w tym samym rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="8ac87-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="8ac87-123">Połączone pliki będą wyglądać następująco w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="8ac87-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="8ac87-124">Należy zauważyć, że plik EDMX jest połączony z akcją kompilacji EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="8ac87-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="8ac87-125">Jest to specjalne zadanie MSBuild (teraz zawarte w pakiecie EF 6,3), które ma na celu dodanie modelu EF do zestawu docelowego jako zasobów osadzonych (lub skopiowanie go jako plików w folderze wyjściowym, w zależności od ustawienia przetwarzania artefaktów metadanych w EDMX).</span><span class="sxs-lookup"><span data-stu-id="8ac87-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="8ac87-126">Aby uzyskać więcej informacji na temat sposobu uzyskania tego ustawienia, zobacz nasze [przykładowe edmx .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="8ac87-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="8ac87-127">Poprzednie wydania</span><span class="sxs-lookup"><span data-stu-id="8ac87-127">Past Releases</span></span>

<span data-ttu-id="8ac87-128">Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="8ac87-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
