---
title: Dostawca bazy danych InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="4c341-102">Dostawca programu EF podstawowej bazy danych w pamięci</span><span class="sxs-lookup"><span data-stu-id="4c341-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="4c341-103">Ten dostawca bazy danych umożliwia Entity Framework Core ma być używany z bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4c341-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="4c341-104">Jest to przydatne podczas testowania kodu korzystającego z programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4c341-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="4c341-105">Dostawca jest przechowywany jako część [projektu EntityFramework GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="4c341-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="4c341-106">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="4c341-106">Install</span></span>

<span data-ttu-id="4c341-107">Zainstaluj [pakietu Microsoft.EntityFrameworkCore.InMemory NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="4c341-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="4c341-108">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="4c341-108">Get Started</span></span>

<span data-ttu-id="4c341-109">Następujące zasoby pomoże Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="4c341-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="4c341-110">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="4c341-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="4c341-111">Przykładowe UnicornStore testów aplikacji</span><span class="sxs-lookup"><span data-stu-id="4c341-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="4c341-112">Obsługiwanych aparatów bazy danych</span><span class="sxs-lookup"><span data-stu-id="4c341-112">Supported Database Engines</span></span>

* <span data-ttu-id="4c341-113">Wbudowane bazy danych w pamięci (przeznaczony tylko do celów testowych)</span><span class="sxs-lookup"><span data-stu-id="4c341-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4c341-114">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="4c341-114">Supported Platforms</span></span>

* <span data-ttu-id="4c341-115">.NET framework (4.5.1 lub nowszej)</span><span class="sxs-lookup"><span data-stu-id="4c341-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="4c341-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c341-116">.NET Core</span></span>

* <span data-ttu-id="4c341-117">Mono (4.2.0 i jego nowszych wersjach)</span><span class="sxs-lookup"><span data-stu-id="4c341-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="4c341-118">Platforma uniwersalna systemu Windows</span><span class="sxs-lookup"><span data-stu-id="4c341-118">Universal Windows Platform</span></span>
