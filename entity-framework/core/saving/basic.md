---
title: Zapisz podstawowy — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a><span data-ttu-id="f09ac-102">Zapisz podstawowy</span><span class="sxs-lookup"><span data-stu-id="f09ac-102">Basic Save</span></span>

<span data-ttu-id="f09ac-103">Informacje o sposobie dodawania, modyfikowania i usuwania danych przy użyciu klas kontekstu i jednostek.</span><span class="sxs-lookup"><span data-stu-id="f09ac-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="f09ac-104">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="f09ac-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="f09ac-105">Dodawanie danych</span><span class="sxs-lookup"><span data-stu-id="f09ac-105">Adding Data</span></span>

<span data-ttu-id="f09ac-106">Użyj *DbSet.Add* metody w celu dodania nowych wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="f09ac-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="f09ac-107">Dane zostanie wstawiony w bazie danych podczas wywoływania *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f09ac-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="f09ac-108">Metody Add, Dołącz i aktualizacji wszystkie działają na wykresie pełne jednostek przekazany do nich, zgodnie z opisem w [dane dotyczące](related-data.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="f09ac-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="f09ac-109">Alternatywnie EntityEntry.State właściwości można ustawić stanu pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="f09ac-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="f09ac-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="f09ac-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="f09ac-111">Aktualizowanie danych</span><span class="sxs-lookup"><span data-stu-id="f09ac-111">Updating Data</span></span>

<span data-ttu-id="f09ac-112">EF automatycznie wykrywa zmiany wprowadzone do istniejącego obiektu, które są śledzone przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="f09ac-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="f09ac-113">W tym jednostki, które możesz obciążenia/zapytania z bazy danych i jednostek, które zostały wcześniej dodane i zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f09ac-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="f09ac-114">Po prostu zmodyfikuj wartości dla właściwości, a następnie wywołać *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f09ac-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="f09ac-115">Usuwanie danych</span><span class="sxs-lookup"><span data-stu-id="f09ac-115">Deleting Data</span></span>

<span data-ttu-id="f09ac-116">Użyj *DbSet.Remove* metodę wystąpienia klas jednostek można usunąć.</span><span class="sxs-lookup"><span data-stu-id="f09ac-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="f09ac-117">Jeśli ta jednostka już istnieje w bazie danych, zostaną usunięte podczas *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f09ac-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="f09ac-118">Jeśli jednostka nie został jeszcze zapisany w bazie danych (np. jego śledzenia dodany), a następnie zostaną usunięte z kontekstu i nie będzie już wstawiony, kiedy *SaveChanges* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f09ac-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="f09ac-119">Wiele operacji w jednej metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f09ac-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="f09ac-120">Można połączyć wiele operacji dodawania/aktualizacji/usuwania w jednym wywołaniu *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f09ac-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="f09ac-121">W przypadku dostawców większości bazy danych *SaveChanges* jest transakcyjna.</span><span class="sxs-lookup"><span data-stu-id="f09ac-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="f09ac-122">Oznacza to, wszystkie operacje będą powodzenie lub Niepowodzenie i operacje będą nigdy nie lewej częściowo stosowane.</span><span class="sxs-lookup"><span data-stu-id="f09ac-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
