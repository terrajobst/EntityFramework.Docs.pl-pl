---
title: Omówienie rdzenia struktury jednostki — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416847"
---
# <a name="entity-framework-core"></a><span data-ttu-id="03d7e-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="03d7e-102">Entity Framework Core</span></span>

<span data-ttu-id="03d7e-103">Entity Framework (EF) Core to lekka, rozszerzalna, [otwarta](https://github.com/aspnet/EntityFrameworkCore) i wieloplatformowa wersja popularnej technologii dostępu do danych entity framework.</span><span class="sxs-lookup"><span data-stu-id="03d7e-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="03d7e-104">EF Core może służyć jako maper obiekt-relacyjne (O/RM), umożliwiając deweloperom platformy .NET do pracy z bazą danych przy użyciu obiektów .NET i eliminując potrzebę większości kodu dostępu do danych, które zwykle muszą napisać.</span><span class="sxs-lookup"><span data-stu-id="03d7e-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="03d7e-105">EF Core obsługuje wiele aparatów baz danych, zobacz [dostawców baz danych, aby](providers/index.md) uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="03d7e-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="03d7e-106">The Model</span><span class="sxs-lookup"><span data-stu-id="03d7e-106">The Model</span></span>

<span data-ttu-id="03d7e-107">Za pomocą EF Core dostęp do danych jest wykonywany przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="03d7e-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="03d7e-108">Model składa się z klas jednostek i obiektu kontekstu, który reprezentuje sesję z bazą danych, umożliwiając wykonywanie zapytań i zapisywanie danych.</span><span class="sxs-lookup"><span data-stu-id="03d7e-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="03d7e-109">Zobacz [Tworzenie modelu,](modeling/index.md) aby dowiedzieć się więcej.</span><span class="sxs-lookup"><span data-stu-id="03d7e-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="03d7e-110">Można wygenerować model z istniejącej bazy danych, kod ręczny modelu, aby dopasować bazę danych lub użyć [ef migracji](managing-schemas/migrations/index.md) do utworzenia bazy danych z modelu, a następnie rozwijać go w miarę zmian modelu w czasie.</span><span class="sxs-lookup"><span data-stu-id="03d7e-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="03d7e-111">Wykonywanie zapytania</span><span class="sxs-lookup"><span data-stu-id="03d7e-111">Querying</span></span>

<span data-ttu-id="03d7e-112">Wystąpienia klas jednostek są pobierane z bazy danych przy użyciu zintegrowanej kwerendy języka (LINQ).</span><span class="sxs-lookup"><span data-stu-id="03d7e-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="03d7e-113">Aby dowiedzieć się więcej, zobacz [Wyszukiwanie danych.](querying/index.md)</span><span class="sxs-lookup"><span data-stu-id="03d7e-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="03d7e-114">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="03d7e-114">Saving Data</span></span>

<span data-ttu-id="03d7e-115">Dane są tworzone, usuwane i modyfikowane w bazie danych przy użyciu wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="03d7e-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="03d7e-116">Aby dowiedzieć się więcej, zobacz [Zapisywanie danych.](saving/index.md)</span><span class="sxs-lookup"><span data-stu-id="03d7e-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="03d7e-117">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="03d7e-117">Next steps</span></span>

<span data-ttu-id="03d7e-118">Aby zapoznać się z samouczkami wprowadzającymi, zobacz [Wprowadzenie do programu Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="03d7e-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
