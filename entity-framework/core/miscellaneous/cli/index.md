---
title: Entity Framework Core odnoszą się narzędzia — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447121"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="ae1d6-102">Dotyczące narzędzi programu Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ae1d6-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="ae1d6-103">Pomoc narzędzia Entity Framework Core przy użyciu zadania podczas projektowania.</span><span class="sxs-lookup"><span data-stu-id="ae1d6-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="ae1d6-104">Są głównie używane do zarządzania, migracji i do tworzenia szkieletu `DbContext` i typów jednostki przez odtwarzanie schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ae1d6-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="ae1d6-105">[Narzędzia Konsola Menedżera pakietów programu EF Core](powershell.md)) uruchom w [Konsola Menedżera pakietów](https://docs.microsoft.com/nuget/tools/package-manager-console) w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ae1d6-105">The [EF Core Package Manager Console tools](powershell.md)) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="ae1d6-106">Te narzędzia działają w projektach .NET Core i .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ae1d6-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="ae1d6-107">[Narzędzi interfejsu wiersza polecenia (CLI) platformy EF Core .NET](dotnet.md) to rozszerzenie dla wielu platform [narzędzi interfejsu wiersza polecenia platformy .NET Core](https://docs.microsoft.com/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="ae1d6-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="ae1d6-108">Narzędzia te wymagają projektu .NET Core SDK (z `Sdk="Microsoft.NET.Sdk"` lub podobne w pliku projektu).</span><span class="sxs-lookup"><span data-stu-id="ae1d6-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="ae1d6-109">Oba narzędzia uwidaczniają taką samą funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="ae1d6-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="ae1d6-110">Jeśli tworzysz w programie Visual Studio, zalecamy użycie **Konsola Menedżera pakietów** narzędzi, ponieważ zapewniają one bardziej zintegrowanego środowiska pracy.</span><span class="sxs-lookup"><span data-stu-id="ae1d6-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae1d6-111">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ae1d6-111">Next steps</span></span>

* [<span data-ttu-id="ae1d6-112">Konsola Menedżera pakietów programu EF Core odnoszą się narzędzia</span><span class="sxs-lookup"><span data-stu-id="ae1d6-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="ae1d6-113">Odwołują się do narzędzia .NET Core interfejsu wiersza polecenia platformy EF</span><span class="sxs-lookup"><span data-stu-id="ae1d6-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)