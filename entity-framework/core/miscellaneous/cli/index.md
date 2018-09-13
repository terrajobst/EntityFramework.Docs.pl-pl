---
title: Informacje dotyczące wiersza polecenia — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490366"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="30f24-102">Entity Framework Core Tools</span><span class="sxs-lookup"><span data-stu-id="30f24-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="30f24-103">Entity Framework Core Tools pomocne podczas tworzenia aplikacji programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="30f24-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="30f24-104">Służą one głównie do tworzenia szkieletu typu DbContext i jednostki przez odtwarzanie schemat bazy danych oraz do zarządzania migracji.</span><span class="sxs-lookup"><span data-stu-id="30f24-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="30f24-105">[Narzędzia konsoli Menedżera pakietów (PMC) EF Core] [ 1] zapewniając doskonałe środowisko w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30f24-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="30f24-106">Uruchamiaj je za pomocą NuGet [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="30f24-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="30f24-107">Te narzędzia działają w projektach .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="30f24-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="30f24-108">[Narzędzia wiersza polecenia platformy .NET Core EF] [ 3] stanowią rozszerzenie do [narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core] [ 4] , które są dla wielu platform i mogą być uruchamiane poza programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30f24-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="30f24-109">Narzędzia te wymagają projektu .NET Core SDK (z `Sdk="Microsoft.NET.Sdk"` lub podobne w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="30f24-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="30f24-110">Oba narzędzia uwidaczniają taką samą funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="30f24-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="30f24-111">Jeśli tworzysz w programie Visual Studio, zaleca się przy użyciu narzędzi konsolę zarządzania Pakietami, ponieważ zapewniają one bardziej zintegrowanego środowiska pracy.</span><span class="sxs-lookup"><span data-stu-id="30f24-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="30f24-112">Struktury</span><span class="sxs-lookup"><span data-stu-id="30f24-112">Frameworks</span></span>
----------
<span data-ttu-id="30f24-113">Narzędzia obsługują projekty przeznaczone dla .NET Framework lub .NET Core.</span><span class="sxs-lookup"><span data-stu-id="30f24-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="30f24-114">Jeśli chcesz użyć biblioteki klas, następnie należy wziąć pod uwagę przy użyciu biblioteki klas platformy .NET Core lub .NET Framework, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="30f24-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="30f24-115">Spowoduje to co najmniej problemów za pomocą narzędzi platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="30f24-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="30f24-116">Jeśli zamiast tego chcesz użyć biblioteki klas .NET Standard, następnie należy użyć projekt startowy platformy .NET Framework lub .NET Core tak, aby narzędzi conrete platformę docelową, do którego można załadować biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="30f24-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="30f24-117">Ten projekt startowy może być fikcyjnego projektu bez rzeczywistego kodu — jest to tylko potrzebne do udostępniać miejsce docelowe dla narzędzi.</span><span class="sxs-lookup"><span data-stu-id="30f24-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="30f24-118">Jeśli projekt jest przeznaczony dla innej framework (na przykład, Universal Windows lub platformy Xamarin), następnie należy utworzyć oddzielne biblioteki klas .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="30f24-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="30f24-119">W takim przypadku postępuj zgodnie z wskazówki powyżej, aby również utworzyć projekt startowy, który może być używany przez narzędzi.</span><span class="sxs-lookup"><span data-stu-id="30f24-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="30f24-120">Uruchamianie i projektów docelowych</span><span class="sxs-lookup"><span data-stu-id="30f24-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="30f24-121">Zawsze, gdy wywołuje polecenie zaangażowanych dwa projekty: projekt docelowy i projekt startowy.</span><span class="sxs-lookup"><span data-stu-id="30f24-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="30f24-122">Projekt docelowy jest w przypadku, gdy są dodawane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="30f24-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="30f24-123">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="30f24-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="30f24-124">Zarówno projekt docelowy, jak i projekt startowy może być taki sam.</span><span class="sxs-lookup"><span data-stu-id="30f24-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
