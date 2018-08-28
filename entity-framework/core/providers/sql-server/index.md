---
title: Dostawca bazy danych serwera programu Microsoft SQL - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995672"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="7fd5f-102">Dostawca bazy danych programu Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="7fd5f-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="7fd5f-103">Ten dostawca bazy danych umożliwia platformy Entity Framework Core do użycia z programem Microsoft SQL Server (w tym usługi SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="7fd5f-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="7fd5f-104">Dostawca jest przechowywane jako część [projektu programu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="7fd5f-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="7fd5f-105">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="7fd5f-105">Install</span></span>

<span data-ttu-id="7fd5f-106">Zainstaluj [pakietu Microsoft.EntityFrameworkCore.SqlServer NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="7fd5f-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="7fd5f-107">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="7fd5f-107">Get Started</span></span>

<span data-ttu-id="7fd5f-108">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="7fd5f-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="7fd5f-109">Rozpoczynanie pracy w programie .NET Framework (Konsola, WinForms, WPF, itd.)</span><span class="sxs-lookup"><span data-stu-id="7fd5f-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="7fd5f-110">Wprowadzenie do programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fd5f-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="7fd5f-111">UnicornStore przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="7fd5f-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="7fd5f-112">Obsługiwane baz danych</span><span class="sxs-lookup"><span data-stu-id="7fd5f-112">Supported Database Engines</span></span>

* <span data-ttu-id="7fd5f-113">Microsoft SQL Server (2008 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="7fd5f-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7fd5f-114">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="7fd5f-114">Supported Platforms</span></span>

* <span data-ttu-id="7fd5f-115">.NET framework (4.5.1 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="7fd5f-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7fd5f-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fd5f-116">.NET Core</span></span>

* <span data-ttu-id="7fd5f-117">Narzędzie mono (4.2.0 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="7fd5f-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
