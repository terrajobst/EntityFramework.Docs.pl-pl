---
title: Informacje dotyczące wiersza polecenia - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769420"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="a2692-102">Entity Framework podstawowe narzędzia</span><span class="sxs-lookup"><span data-stu-id="a2692-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="a2692-103">Entity Framework podstawowe narzędzia pomóc podczas opracowywania aplikacji EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2692-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="a2692-104">Aby utworzyć szkielet typy DbContext i jednostki przez odtwarzanie schematu bazy danych oraz do zarządzania migracji są używane przede wszystkim.</span><span class="sxs-lookup"><span data-stu-id="a2692-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="a2692-105">[Konsoli Menedżera pakietów (PMC) EF podstawowe narzędzia] [ 1] zapewnienie wyższego poziomu środowiska w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2692-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="a2692-106">Uruchom je przy użyciu narzędzia NuGet [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="a2692-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="a2692-107">Te narzędzia Praca z projektami zarówno .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2692-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="a2692-108">[Narzędzia wiersza polecenia platformy .NET Core EF] [ 3] są rozszerzeniem [narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core] [ 4] , które są obsługujący wiele platform i mogą uruchamiać poza Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2692-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="a2692-109">Narzędzia te wymagają projekt zestawu SDK programu .NET Core (jeden z `Sdk="Microsoft.NET.Sdk"` lub podobne w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="a2692-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="a2692-110">Zarówno narzędzia uwidaczniają te same funkcje.</span><span class="sxs-lookup"><span data-stu-id="a2692-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="a2692-111">Jeśli projektujesz w programie Visual Studio, zaleca się za pomocą narzędzia PMC, ponieważ zapewniają większą integrację.</span><span class="sxs-lookup"><span data-stu-id="a2692-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="a2692-112">Struktury</span><span class="sxs-lookup"><span data-stu-id="a2692-112">Frameworks</span></span>
----------
<span data-ttu-id="a2692-113">Narzędzia obsługi projektach przeznaczonych dla platformy .NET Framework lub .NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2692-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="a2692-114">Jeśli chcesz użyć biblioteki klas, należy rozważyć, jeśli to możliwe przy użyciu biblioteki klas .NET Core lub .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a2692-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="a2692-115">Spowoduje to co najmniej problemy z narzędzi platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="a2692-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="a2692-116">Jeśli zamiast tego chcesz użyć .NET Standard biblioteki klas, następnie należy użyć projekt startowy .NET Framework lub .NET Core tak, aby narzędzi conrete platformy docelowej, do którego można załadować biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="a2692-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="a2692-117">Ten projekt startowy może być fikcyjny projektu z żadnego kodu rzeczywistych — jest wymagana jedynie zapewnienie elementu docelowego dla narzędzia.</span><span class="sxs-lookup"><span data-stu-id="a2692-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="a2692-118">Jeśli projekt jest przeznaczony dla innej framework (na przykład aplikacja uniwersalna systemu Windows lub Xamarin), następnie należy utworzyć oddzielne biblioteki klas .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="a2692-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="a2692-119">W takim przypadku należy wykonać wskazówki powyżej, aby również utworzyć projekt startowy, które mogą być używane przez narzędzia.</span><span class="sxs-lookup"><span data-stu-id="a2692-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="a2692-120">Uruchamianie i projektów docelowych</span><span class="sxs-lookup"><span data-stu-id="a2692-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="a2692-121">Przy każdym wywołaniu polecenia obejmuje dwa projekty: Projekt startowy i projektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="a2692-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="a2692-122">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="a2692-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="a2692-123">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="a2692-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="a2692-124">Zarówno projekt startowy, jak i docelowy projekt może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="a2692-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
