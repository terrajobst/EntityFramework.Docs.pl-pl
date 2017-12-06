---
title: "Bazy danych SQLite dostawcy — ograniczenia - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="b386e-102">Ograniczenia dotyczące dostawcy bazy danych SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="b386e-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="b386e-103">Dostawca bazy danych SQLite ma różne ograniczenia migracji.</span><span class="sxs-lookup"><span data-stu-id="b386e-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="b386e-104">Większość tych ograniczeń wyniku ograniczenia w podstawowej bazy danych SQLite i nie są specyficzne dla EF.</span><span class="sxs-lookup"><span data-stu-id="b386e-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="b386e-105">Ograniczenia modelowania</span><span class="sxs-lookup"><span data-stu-id="b386e-105">Modeling limitations</span></span>

<span data-ttu-id="b386e-106">Wspólna biblioteka relacyjnych (udostępniony przez dostawców relacyjnej bazy danych programu Entity Framework) definiuje interfejsów API do modelowania pojęcia, które są wspólne dla większości relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="b386e-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="b386e-107">Kilka tych pojęć nie są obsługiwane przez dostawcę danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="b386e-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="b386e-108">Schematy</span><span class="sxs-lookup"><span data-stu-id="b386e-108">Schemas</span></span>
* <span data-ttu-id="b386e-109">Sekwencje</span><span class="sxs-lookup"><span data-stu-id="b386e-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="b386e-110">Ograniczenia migracji</span><span class="sxs-lookup"><span data-stu-id="b386e-110">Migrations limitations</span></span>

<span data-ttu-id="b386e-111">Aparat bazy danych SQLite nie obsługuje wielu operacji schematu, które są obsługiwane przez większość innych relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="b386e-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="b386e-112">Jeśli spróbujesz dotyczą jednej z nieobsługiwanej operacji bazy danych SQLite, a następnie `NotSupportedException` zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="b386e-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="b386e-113">Operacja</span><span class="sxs-lookup"><span data-stu-id="b386e-113">Operation</span></span>            | <span data-ttu-id="b386e-114">Obsługiwane?</span><span class="sxs-lookup"><span data-stu-id="b386e-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="b386e-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="b386e-115">AddColumn</span></span>            | <span data-ttu-id="b386e-116">✔</span><span class="sxs-lookup"><span data-stu-id="b386e-116">✔</span></span>          |
| <span data-ttu-id="b386e-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="b386e-117">AddForeignKey</span></span>        | <span data-ttu-id="b386e-118">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-118">✗</span></span>          |
| <span data-ttu-id="b386e-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="b386e-119">AddPrimaryKey</span></span>        | <span data-ttu-id="b386e-120">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-120">✗</span></span>          |
| <span data-ttu-id="b386e-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="b386e-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="b386e-122">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-122">✗</span></span>          |
| <span data-ttu-id="b386e-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="b386e-123">AlterColumn</span></span>          | <span data-ttu-id="b386e-124">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-124">✗</span></span>          |
| <span data-ttu-id="b386e-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="b386e-125">CreateIndex</span></span>          | <span data-ttu-id="b386e-126">✔</span><span class="sxs-lookup"><span data-stu-id="b386e-126">✔</span></span>          |
| <span data-ttu-id="b386e-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="b386e-127">CreateTable</span></span>          | <span data-ttu-id="b386e-128">✔</span><span class="sxs-lookup"><span data-stu-id="b386e-128">✔</span></span>          |
| <span data-ttu-id="b386e-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="b386e-129">DropColumn</span></span>           | <span data-ttu-id="b386e-130">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-130">✗</span></span>          |
| <span data-ttu-id="b386e-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="b386e-131">DropForeignKey</span></span>       | <span data-ttu-id="b386e-132">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-132">✗</span></span>          |
| <span data-ttu-id="b386e-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="b386e-133">DropIndex</span></span>            | <span data-ttu-id="b386e-134">✔</span><span class="sxs-lookup"><span data-stu-id="b386e-134">✔</span></span>          |
| <span data-ttu-id="b386e-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="b386e-135">DropPrimaryKey</span></span>       | <span data-ttu-id="b386e-136">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-136">✗</span></span>          |
| <span data-ttu-id="b386e-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="b386e-137">DropTable</span></span>            | <span data-ttu-id="b386e-138">✔</span><span class="sxs-lookup"><span data-stu-id="b386e-138">✔</span></span>          |
| <span data-ttu-id="b386e-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="b386e-139">DropUniqueConstraint</span></span> | <span data-ttu-id="b386e-140">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-140">✗</span></span>          |
| <span data-ttu-id="b386e-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="b386e-141">RenameColumn</span></span>         | <span data-ttu-id="b386e-142">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-142">✗</span></span>          |
| <span data-ttu-id="b386e-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="b386e-143">RenameIndex</span></span>          | <span data-ttu-id="b386e-144">✗</span><span class="sxs-lookup"><span data-stu-id="b386e-144">✗</span></span>          |
| <span data-ttu-id="b386e-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="b386e-145">RenameTable</span></span>          | <span data-ttu-id="b386e-146">✔</span><span class="sxs-lookup"><span data-stu-id="b386e-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="b386e-147">Obejście ograniczenia dotyczące migracji</span><span class="sxs-lookup"><span data-stu-id="b386e-147">Migrations limitations workaround</span></span>

<span data-ttu-id="b386e-148">Niektóre można obejść tych ograniczeń ręcznie pisanie kodu w migracji do wykonania tabeli odbudować.</span><span class="sxs-lookup"><span data-stu-id="b386e-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="b386e-149">Odbuduj tabelę obejmuje zmianę nazwy istniejącego tabeli, tworzy nową tabelę kopiowanie danych do nowej tabeli i usunięcie starej tabeli.</span><span class="sxs-lookup"><span data-stu-id="b386e-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="b386e-150">Będą musieli używać `Sql(string)` metody do wykonania niektórych z tych kroków.</span><span class="sxs-lookup"><span data-stu-id="b386e-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="b386e-151">Zobacz [co inne rodzaje z zmiany schematu tabeli](http://sqlite.org/lang_altertable.html#otheralter) w dokumentacji SQLite, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="b386e-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="b386e-152">W przyszłości EF może obsługiwać niektóre z tych operacji przy użyciu podejścia Odbuduj tabelę w obszarze obejmuje.</span><span class="sxs-lookup"><span data-stu-id="b386e-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="b386e-153">Możesz [śledzenie tej funkcji w naszym projektu GitHub](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="b386e-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
