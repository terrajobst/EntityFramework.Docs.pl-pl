---
title: "Informacje dotyczące wiersza polecenia - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="4da95-102">Entity Framework podstawowe narzędzia</span><span class="sxs-lookup"><span data-stu-id="4da95-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="4da95-103">Entity Framework podstawowe narzędzia pomóc podczas opracowywania aplikacji EF Core.</span><span class="sxs-lookup"><span data-stu-id="4da95-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="4da95-104">Aby utworzyć szkielet typy DbContext i jednostki przez odtwarzanie schematu bazy danych oraz do zarządzania migracji są używane przede wszystkim.</span><span class="sxs-lookup"><span data-stu-id="4da95-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="4da95-105">[Konsoli Menedżera pakietów (PMC) EF podstawowe narzędzia] [ 1] zapewnienie wyższego poziomu środowiska w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4da95-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="4da95-106">Uruchom je przy użyciu narzędzia NuGet [Konsola Menedżera pakietów][2].</span><span class="sxs-lookup"><span data-stu-id="4da95-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="4da95-107">Te narzędzia Praca z projektami zarówno .NET Framework i .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4da95-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="4da95-108">[Narzędzia wiersza polecenia platformy .NET Core EF] [ 3] są rozszerzeniem [narzędzi interfejsu wiersza polecenia (CLI) platformy .NET Core] [ 4] , które są obsługujący wiele platform i mogą uruchamiać poza Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4da95-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="4da95-109">Narzędzia te wymagają projekt zestawu SDK programu .NET Core (jeden z `Sdk="Microsoft.NET.Sdk"` lub podobne w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="4da95-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="4da95-110">Zarówno narzędzia uwidaczniają te same funkcje.</span><span class="sxs-lookup"><span data-stu-id="4da95-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="4da95-111">Jeśli projektujesz w programie Visual Studio, zaleca się za pomocą narzędzia PMC, ponieważ zapewniają większą integrację.</span><span class="sxs-lookup"><span data-stu-id="4da95-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="4da95-112">Struktury</span><span class="sxs-lookup"><span data-stu-id="4da95-112">Frameworks</span></span>
----------
<span data-ttu-id="4da95-113">Narzędzia obsługi projektach przeznaczonych dla platformy .NET Framework lub .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4da95-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="4da95-114">Jeśli projekt jest przeznaczony dla innej framework (na przykład aplikacja uniwersalna systemu Windows lub Xamarin), zaleca się utworzenie oddzielnej .NET Standard projektów i elementów docelowych między jedną z obsługiwanych platform.</span><span class="sxs-lookup"><span data-stu-id="4da95-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="4da95-115">Docelowe między .NET Core, na przykład, kliknij prawym przyciskiem myszy projekt i wybierz **Edytuj \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="4da95-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="4da95-116">Aktualizacja `TargetFramework` właściwości w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="4da95-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="4da95-117">(Należy pamiętać, nazwa właściwości staje się w liczbie mnogiej.)</span><span class="sxs-lookup"><span data-stu-id="4da95-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="4da95-118">Jeśli używasz programu .NET Standard biblioteki klas, nie trzeba między docelowych, jeśli Twój projekt startowy jest przeznaczony dla platformy .NET Framework lub .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4da95-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="4da95-119">Uruchamianie i projektów docelowych</span><span class="sxs-lookup"><span data-stu-id="4da95-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="4da95-120">Przy każdym wywołaniu polecenia obejmuje dwa projekty: Projekt startowy i projektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="4da95-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="4da95-121">Projekt docelowy jest w przypadku, gdy zostaną dodane wszystkie pliki (lub w niektórych przypadkach usunięte).</span><span class="sxs-lookup"><span data-stu-id="4da95-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="4da95-122">Projekt startowy jest emulowane przez narzędzia podczas wykonywania kodu projektu.</span><span class="sxs-lookup"><span data-stu-id="4da95-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="4da95-123">Zarówno projekt startowy, jak i docelowy projekt może być taka sama.</span><span class="sxs-lookup"><span data-stu-id="4da95-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
