---
title: Pisanie dostawcy bazy danych — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993669"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="92c42-102">Pisanie dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="92c42-102">Writing a Database Provider</span></span>

<span data-ttu-id="92c42-103">Informacje na temat pisania dostawcy bazy danych programu Entity Framework Core, zobacz [więc chcesz napisać dostawcy programu EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) przez [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="92c42-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="92c42-104">Te wpisy nie zostały zaktualizowane od czasu programu EF Core 1.1 i od tego czasu miały miejsce znaczące zmiany [681 problem](https://github.com/aspnet/EntityFramework.Docs/issues/681) śledzenie aktualizacji tej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="92c42-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="92c42-105">Codebase programu EF Core jest "open source" i zawiera kilka dostawcy baz danych, które mogą służyć jako odwołanie.</span><span class="sxs-lookup"><span data-stu-id="92c42-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="92c42-106">Można znaleźć kodu źródłowego w https://github.com/aspnet/EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="92c42-106">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span> <span data-ttu-id="92c42-107">Może to być również przydatne Spójrz na kod dla często używanych dostawców innych firm, takich jak [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), i [programu SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="92c42-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="92c42-108">W szczególności te projekty są Instalatora, aby rozszerzyć i uruchom testy funkcjonalne, które możemy opublikować dla narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="92c42-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="92c42-109">Zdecydowanie zaleca się tego rodzaju instalacji.</span><span class="sxs-lookup"><span data-stu-id="92c42-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="92c42-110">Aktualizowanie zmian w dostawcy</span><span class="sxs-lookup"><span data-stu-id="92c42-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="92c42-111">Po wydaniu wersji 2.1, zaczynając od pracy, firma Microsoft opracowała [dziennika zmian](provider-log.md) , może być konieczne odpowiednie zmiany w kodzie dostawcy.</span><span class="sxs-lookup"><span data-stu-id="92c42-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="92c42-112">Ta wartość jest przeznaczona do pomocy podczas aktualizowania istniejącego dostawcy do pracy z nową wersję programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="92c42-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="92c42-113">Przed 2.1, użyliśmy [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) i [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etykiety na naszych problemy usługi GitHub i żądań ściągnięcia dla podobnych celów.</span><span class="sxs-lookup"><span data-stu-id="92c42-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="92c42-114">Firma Microsoft będzie continiue na potrzeby tych lables na problemy stanowić wskazówkę, które elementy robocze w danej wersji może również wymagać pracy w dostawcy.</span><span class="sxs-lookup"><span data-stu-id="92c42-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="92c42-115">A `providers-beware` etykiety zazwyczaj oznacza, że implementacja elementu roboczego może spowodować przerwanie dostawców, podczas gdy `providers-fyi` etykiety zazwyczaj oznacza, że dostawcy nie będą działać, ale kod może być konieczne, należy zmienić mimo to, na przykład, aby włączyć nowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="92c42-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="92c42-116">Sugerowane nazewnictwa dostawców innych firm</span><span class="sxs-lookup"><span data-stu-id="92c42-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="92c42-117">Zaleca się przy użyciu następujących nazewnictwa dla pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="92c42-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="92c42-118">Jest to zgodne z nazwami pakiety dostarczone przez zespół programu EF Core.</span><span class="sxs-lookup"><span data-stu-id="92c42-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="92c42-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="92c42-119">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
