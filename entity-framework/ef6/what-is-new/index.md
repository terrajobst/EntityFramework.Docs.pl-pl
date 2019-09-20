---
title: Co nowego — EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149125"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="f7b69-102">Co nowego w programie EF6</span><span class="sxs-lookup"><span data-stu-id="f7b69-102">What's New in EF6</span></span>

<span data-ttu-id="f7b69-103">Zdecydowanie zalecamy użycie najnowszej wydanej wersji Entity Framework, aby uzyskać najnowsze funkcje i największą stabilność.</span><span class="sxs-lookup"><span data-stu-id="f7b69-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="f7b69-104">Jednak firma Microsoft zdaje sobie sprawę, że może być konieczne użycie poprzedniej wersji lub przeprowadzenie eksperymentu z nowymi ulepszeniami w najnowszej wersji wstępnej.</span><span class="sxs-lookup"><span data-stu-id="f7b69-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="f7b69-105">Aby zainstalować określone wersje EF, zobacz [Get Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="f7b69-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="f7b69-106">DR 6.3.0</span><span class="sxs-lookup"><span data-stu-id="f7b69-106">EF 6.3.0</span></span>

<span data-ttu-id="f7b69-107">Środowisko uruchomieniowe EF 6.3.0 zostało wydane dla programu NuGet we wrześniu 2019.</span><span class="sxs-lookup"><span data-stu-id="f7b69-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="f7b69-108">Głównym celem tej wersji jest ułatwienie migracji istniejących aplikacji korzystających z programu EF 6 do platformy .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="f7b69-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="f7b69-109">Społeczność przyczyniła również kilka poprawek i ulepszeń błędów.</span><span class="sxs-lookup"><span data-stu-id="f7b69-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="f7b69-110">Aby uzyskać szczegółowe informacje, zobacz problemy zamknięte w każdym 6.3.0 [punktów kontrolnych](https://github.com/aspnet/EntityFramework6/milestones?state=closed) .</span><span class="sxs-lookup"><span data-stu-id="f7b69-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="f7b69-111">Poniżej przedstawiono niektóre z bardziej istotnych:</span><span class="sxs-lookup"><span data-stu-id="f7b69-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="f7b69-112">Obsługa platformy .NET Core 3,0</span><span class="sxs-lookup"><span data-stu-id="f7b69-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="f7b69-113">Pakiet EntityFramework teraz jest przeznaczony dla .NET Standard 2,1 oprócz .NET Framework 4. x</span><span class="sxs-lookup"><span data-stu-id="f7b69-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="f7b69-114">Polecenia migracji zostały wprowadzone ponownie w celu wykonania poza procesem i pracy z projektami w stylu zestawu SDK</span><span class="sxs-lookup"><span data-stu-id="f7b69-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="f7b69-115">Obsługa SQL Server HierarchyId</span><span class="sxs-lookup"><span data-stu-id="f7b69-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="f7b69-116">Ulepszona zgodność z Roslyn i NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="f7b69-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="f7b69-117">Dodano program Ef6. exe na potrzeby włączania, dodawania, tworzenia skryptów i stosowania migracji z zestawów.</span><span class="sxs-lookup"><span data-stu-id="f7b69-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="f7b69-118">Spowoduje to zastąpienie migracji. exe</span><span class="sxs-lookup"><span data-stu-id="f7b69-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="f7b69-119">Wcześniejsze wersje</span><span class="sxs-lookup"><span data-stu-id="f7b69-119">Past Releases</span></span>

<span data-ttu-id="f7b69-120">Strona [przeszłe wydania](past-releases.md) zawiera archiwum wszystkich poprzednich wersji EF oraz głównych funkcji wprowadzonych w poszczególnych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="f7b69-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
