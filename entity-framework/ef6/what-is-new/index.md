---
title: Co nowego — EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136139"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="8d402-102">Co nowego w programie EF6</span><span class="sxs-lookup"><span data-stu-id="8d402-102">What's new in EF6</span></span>

<span data-ttu-id="8d402-103">Zdecydowanie zalecamy użycie najnowszej wydanej wersji Entity Framework, aby uzyskać najnowsze funkcje i największą stabilność.</span><span class="sxs-lookup"><span data-stu-id="8d402-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="8d402-104">Jednak firma Microsoft zdaje sobie sprawę, że może być konieczne użycie poprzedniej wersji lub przeprowadzenie eksperymentu z nowymi ulepszeniami w najnowszej wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="8d402-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="8d402-105">Aby zainstalować określone wersje EF, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="8d402-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="8d402-106">DR 6.4.0</span><span class="sxs-lookup"><span data-stu-id="8d402-106">EF 6.4.0</span></span>

<span data-ttu-id="8d402-107">Środowisko uruchomieniowe EF 6.4.0 zostało wydane w programie NuGet w grudniu 2019.</span><span class="sxs-lookup"><span data-stu-id="8d402-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="8d402-108">Głównym celem EF 6,4 jest polerowanie funkcji i scenariuszy, które zostały dostarczone w EF 6,3.</span><span class="sxs-lookup"><span data-stu-id="8d402-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="8d402-109">Zobacz [listę ważnych poprawek](https://github.com/dotnet/ef6/milestone/14?closed=1) w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="8d402-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="8d402-110">DR 6.3.0</span><span class="sxs-lookup"><span data-stu-id="8d402-110">EF 6.3.0</span></span>

<span data-ttu-id="8d402-111">Środowisko uruchomieniowe EF 6.3.0 zostało wydane dla programu NuGet we wrześniu 2019.</span><span class="sxs-lookup"><span data-stu-id="8d402-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="8d402-112">Głównym celem tej wersji jest ułatwienie migracji istniejących aplikacji korzystających z programu EF 6 do platformy .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="8d402-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="8d402-113">Społeczność przyczyniła również kilka poprawek i ulepszeń błędów.</span><span class="sxs-lookup"><span data-stu-id="8d402-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="8d402-114">Aby uzyskać szczegółowe informacje, zobacz problemy zamknięte w każdym 6.3.0 [punktów kontrolnych](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="8d402-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="8d402-115">Poniżej przedstawiono niektóre z bardziej istotnych:</span><span class="sxs-lookup"><span data-stu-id="8d402-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="8d402-116">Obsługa platformy .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="8d402-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="8d402-117">Pakiet EntityFramework teraz jest przeznaczony dla .NET Standard 2,1 oprócz .NET Framework 4. x.</span><span class="sxs-lookup"><span data-stu-id="8d402-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="8d402-118">Oznacza to, że EF 6,3 jest dla wielu platform i jest obsługiwany przez inne systemy operacyjne niż Windows, takie jak Linux i macOS.</span><span class="sxs-lookup"><span data-stu-id="8d402-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="8d402-119">Polecenia migracji zostały wprowadzone ponownie w celu wykonania poza procesem i pracują z projektami w stylu zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="8d402-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="8d402-120">Obsługa SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="8d402-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="8d402-121">Ulepszona zgodność z Roslyn i NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="8d402-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="8d402-122">Dodano narzędzie `ef6.exe` do włączania, dodawania, tworzenia skryptów i stosowania migracji z zestawów.</span><span class="sxs-lookup"><span data-stu-id="8d402-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="8d402-123">Spowoduje to zastąpienie `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="8d402-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="8d402-124">Obsługa projektanta EF</span><span class="sxs-lookup"><span data-stu-id="8d402-124">EF designer support</span></span>

<span data-ttu-id="8d402-125">Nie jest obecnie obsługiwane korzystanie z programu EF Designer bezpośrednio w projektach .NET Core lub .NET Standard projects lub w .NET Framework projekcie w stylu zestawu SDK.</span><span class="sxs-lookup"><span data-stu-id="8d402-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="8d402-126">To ograniczenie można obejść przez dodanie pliku EDMX i wygenerowanych klas dla jednostek i DbContext jako połączonych plików do projektu .NET Core 3,0 lub .NET Standard 2,1 w tym samym rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="8d402-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="8d402-127">Połączone pliki będą wyglądać następująco w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="8d402-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="8d402-128">Należy zauważyć, że plik EDMX jest połączony z akcją kompilacji EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="8d402-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="8d402-129">Jest to specjalne zadanie MSBuild (teraz zawarte w pakiecie EF 6,3), które ma na celu dodanie modelu EF do zestawu docelowego jako zasobów osadzonych (lub skopiowanie go jako plików w folderze wyjściowym, w zależności od ustawienia przetwarzania artefaktów metadanych w EDMX).</span><span class="sxs-lookup"><span data-stu-id="8d402-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="8d402-130">Aby uzyskać więcej informacji na temat sposobu uzyskania tego ustawienia, zobacz nasze [przykładowe edmx .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="8d402-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="8d402-131">Ostrzeżenie: Upewnij się, że stary styl (tj. styl inny niż zestaw SDK) .NET Framework projektu definiującego plik "Real". edmx jest _wcześniejszy niż_ projekt definiujący łącze wewnątrz pliku. sln.</span><span class="sxs-lookup"><span data-stu-id="8d402-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="8d402-132">W przeciwnym razie po otwarciu pliku. edmx w projektancie zostanie wyświetlony komunikat o błędzie "Entity Framework nie jest dostępny w platformie docelowej aktualnie określonej dla projektu.</span><span class="sxs-lookup"><span data-stu-id="8d402-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="8d402-133">Można zmienić platformę docelową projektu lub edytować model w obiekcie xmlediter.</span><span class="sxs-lookup"><span data-stu-id="8d402-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="8d402-134">Poprzednie wydania</span><span class="sxs-lookup"><span data-stu-id="8d402-134">Past Releases</span></span>

<span data-ttu-id="8d402-135">Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="8d402-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
