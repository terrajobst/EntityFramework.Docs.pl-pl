---
title: Dostawca bazy danych inMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149222"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="caa4e-102">EF Core dostawcy bazy danych w pamięci</span><span class="sxs-lookup"><span data-stu-id="caa4e-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="caa4e-103">Ten dostawca bazy danych umożliwia Entity Framework Core używany z bazą danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="caa4e-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="caa4e-104">Może to być przydatne do testowania, chociaż dostawca programu SQLite w trybie w pamięci może być bardziej odpowiednim zamiennikiem testu dla relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="caa4e-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="caa4e-105">Dostawca jest obsługiwany w ramach [projektu Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="caa4e-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="caa4e-106">Zainstaluj</span><span class="sxs-lookup"><span data-stu-id="caa4e-106">Install</span></span>

<span data-ttu-id="caa4e-107">Zainstaluj [pakiet NuGet Microsoft. EntityFrameworkCore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="caa4e-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="caa4e-108">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="caa4e-108">Get Started</span></span>

<span data-ttu-id="caa4e-109">Poniższe zasoby pomogą Ci rozpocząć pracę z tym dostawcą.</span><span class="sxs-lookup"><span data-stu-id="caa4e-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="caa4e-110">Testowanie za pomocą InMemory</span><span class="sxs-lookup"><span data-stu-id="caa4e-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="caa4e-111">Testy przykładowej aplikacji UnicornStore</span><span class="sxs-lookup"><span data-stu-id="caa4e-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="caa4e-112">Obsługiwane aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="caa4e-112">Supported Database Engines</span></span>

* <span data-ttu-id="caa4e-113">Wbudowana baza danych w pamięci (przeznaczona tylko do celów testowych)</span><span class="sxs-lookup"><span data-stu-id="caa4e-113">Built-in in-memory database (designed for testing purposes only)</span></span>
