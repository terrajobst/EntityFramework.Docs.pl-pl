---
title: Dostawca InMemory bazy danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993323"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="2cdaf-102">Dostawca programu EF Core bazy danych w pamięci</span><span class="sxs-lookup"><span data-stu-id="2cdaf-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="2cdaf-103">Ten dostawca bazy danych umożliwia platformy Entity Framework Core ma być używany z bazą danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="2cdaf-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="2cdaf-104">Może to być przydatne w przypadku testowania, mimo że dostawcy bazy danych SQLite w trybie w pamięci może być bardziej odpowiednie zastąpienia testu dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="2cdaf-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="2cdaf-105">Dostawca jest przechowywane jako część [projektu programu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2cdaf-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2cdaf-106">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="2cdaf-106">Install</span></span>

<span data-ttu-id="2cdaf-107">Zainstaluj [pakietu Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="2cdaf-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="2cdaf-108">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="2cdaf-108">Get Started</span></span>

<span data-ttu-id="2cdaf-109">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="2cdaf-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="2cdaf-110">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="2cdaf-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="2cdaf-111">Przykładowe UnicornStore testów aplikacji</span><span class="sxs-lookup"><span data-stu-id="2cdaf-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="2cdaf-112">Obsługiwane baz danych</span><span class="sxs-lookup"><span data-stu-id="2cdaf-112">Supported Database Engines</span></span>

* <span data-ttu-id="2cdaf-113">Wbudowane bazy danych w pamięci (przeznaczony tylko do celów testowych)</span><span class="sxs-lookup"><span data-stu-id="2cdaf-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2cdaf-114">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="2cdaf-114">Supported Platforms</span></span>

* <span data-ttu-id="2cdaf-115">.NET framework (4.5.1 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="2cdaf-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="2cdaf-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cdaf-116">.NET Core</span></span>

* <span data-ttu-id="2cdaf-117">Narzędzie mono (4.2.0 lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="2cdaf-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="2cdaf-118">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="2cdaf-118">Universal Windows Platform</span></span>
