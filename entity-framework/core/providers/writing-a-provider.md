---
title: "Pisanie dostawca bazy danych — podstawowe EF"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="44cf0-102">Pisanie dostawcy bazy danych</span><span class="sxs-lookup"><span data-stu-id="44cf0-102">Writing a Database Provider</span></span>

<span data-ttu-id="44cf0-103">Informacje na temat pisania dostawcy bazy danych programu Entity Framework Core, zobacz [takim razie chcesz zapisać dostawcę EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) przez [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="44cf0-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="44cf0-104">Kod EF Core podstawowy jest typu open source i zawiera kilka dostawcy bazy danych, które mogą służyć jako odwołanie.</span><span class="sxs-lookup"><span data-stu-id="44cf0-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="44cf0-105">Kod źródłowy można znaleźć w https://github.com/aspnet/EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="44cf0-105">You can find the source code at https://github.com/aspnet/EntityFramework.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="44cf0-106">Etykieta Uważaj dostawców</span><span class="sxs-lookup"><span data-stu-id="44cf0-106">The providers-beware label</span></span>

<span data-ttu-id="44cf0-107">Gdy rozpoczniesz pracę nad dostawcę obserwować [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) etykiety na naszych GitHub problemy i żądania ściągnięcia.</span><span class="sxs-lookup"><span data-stu-id="44cf0-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFramework/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="44cf0-108">Etykieta możemy służy do identyfikowania zmiany, które mogą mieć wpływ na dostawcy modułów zapisujących.</span><span class="sxs-lookup"><span data-stu-id="44cf0-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="44cf0-109">Sugerowane nazwy dostawców innych firm</span><span class="sxs-lookup"><span data-stu-id="44cf0-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="44cf0-110">Zaleca się przy użyciu następujących nazewnictwa dla pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="44cf0-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="44cf0-111">Jest to zgodne z nazwami paczek przez zespół EF Core.</span><span class="sxs-lookup"><span data-stu-id="44cf0-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="44cf0-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="44cf0-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
