---
title: Dostawca bazy danych InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678989"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="37100-102">Dostawca programu EF podstawowej bazy danych w pamięci</span><span class="sxs-lookup"><span data-stu-id="37100-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="37100-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="37100-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="37100-104">Może to być przydatne podczas testowania, chociaż dostawcy bazy danych SQLite w trybie w pamięci może być bardziej odpowiednie zastąpienia testu relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="37100-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="37100-105">Dostawca jest przechowywany jako część [Entity Framework Core projektu](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="37100-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="37100-106">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="37100-106">Install</span></span>

<span data-ttu-id="37100-107">Zainstaluj [pakietu Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="37100-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="37100-108">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="37100-108">Get Started</span></span>

<span data-ttu-id="37100-109">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="37100-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="37100-110">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="37100-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="37100-111">Przykładowe UnicornStore testów aplikacji</span><span class="sxs-lookup"><span data-stu-id="37100-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="37100-112">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="37100-112">Supported Database Engines</span></span>

* <span data-ttu-id="37100-113">Wbudowane bazy danych w pamięci (przeznaczony tylko do celów testowych)</span><span class="sxs-lookup"><span data-stu-id="37100-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="37100-114">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="37100-114">Supported Platforms</span></span>

* <span data-ttu-id="37100-115">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="37100-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="37100-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="37100-116">.NET Core</span></span>

* <span data-ttu-id="37100-117">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="37100-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="37100-118">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="37100-118">Universal Windows Platform</span></span>
