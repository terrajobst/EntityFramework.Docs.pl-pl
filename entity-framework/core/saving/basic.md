---
title: Zapisywanie podstawowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 6f72458504a9dbe99038af7cfd23b6991258f6b8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197779"
---
# <a name="basic-save"></a><span data-ttu-id="c5cf9-102">Zapisywanie podstawowe</span><span class="sxs-lookup"><span data-stu-id="c5cf9-102">Basic Save</span></span>

<span data-ttu-id="c5cf9-103">Dowiedz się, jak dodawać, modyfikować i usuwać dane przy użyciu własnych klas kontekstu i jednostek.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="c5cf9-104">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="c5cf9-105">Dodawanie danych</span><span class="sxs-lookup"><span data-stu-id="c5cf9-105">Adding Data</span></span>

<span data-ttu-id="c5cf9-106">Użyj metody *nieogólnymi. Add* , aby dodać nowe wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="c5cf9-107">Dane zostaną wstawione do bazy danych po wywołaniu *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="c5cf9-108">Metody dodawania, dołączania i aktualizowania działają na pełnym wykresie obiektów, do których przekazały, zgodnie z opisem w sekcji [powiązane dane](related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="c5cf9-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="c5cf9-109">Alternatywnie Właściwość EntityEntry. State może służyć do ustawiania stanu tylko pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="c5cf9-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="c5cf9-111">Aktualizowanie danych</span><span class="sxs-lookup"><span data-stu-id="c5cf9-111">Updating Data</span></span>

<span data-ttu-id="c5cf9-112">EF automatycznie wykrywa zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="c5cf9-113">Obejmuje to jednostki, które zostały załadowane z bazy danych, oraz obiekty, które zostały wcześniej dodane i zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="c5cf9-114">Po prostu Zmodyfikuj wartości przypisane do właściwości, a następnie Wywołaj *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="c5cf9-115">Usuwanie danych</span><span class="sxs-lookup"><span data-stu-id="c5cf9-115">Deleting Data</span></span>

<span data-ttu-id="c5cf9-116">Użyj metody *nieogólnymi. Remove* , aby usunąć wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="c5cf9-117">Jeśli jednostka już istnieje w bazie danych, zostanie usunięta podczas *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="c5cf9-118">Jeśli obiekt nie został jeszcze zapisany w bazie danych (jest śledzony jako dodany), zostanie usunięty z kontekstu i nie będzie już wstawiany, gdy *metody SaveChanges* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="c5cf9-119">Wiele operacji w jednym metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="c5cf9-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="c5cf9-120">Można połączyć wiele operacji dodawania/aktualizowania/usuwania w jedno wywołanie do *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="c5cf9-121">W przypadku większości dostawców baz danych *metody SaveChanges* jest transakcyjna.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="c5cf9-122">Oznacza to, że wszystkie operacje zakończą się powodzeniem lub niepowodzeniem, a operacje nigdy nie zostaną zastosowane częściowo.</span><span class="sxs-lookup"><span data-stu-id="c5cf9-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
