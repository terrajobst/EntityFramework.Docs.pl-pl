---
title: Zapisywanie podstawowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417636"
---
# <a name="basic-save"></a><span data-ttu-id="11903-102">Zapisywanie podstawowe</span><span class="sxs-lookup"><span data-stu-id="11903-102">Basic Save</span></span>

<span data-ttu-id="11903-103">Dowiedz się, jak dodawać, modyfikować i usuwać dane przy użyciu własnych klas kontekstu i jednostek.</span><span class="sxs-lookup"><span data-stu-id="11903-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="11903-104">[Przykład](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="11903-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="11903-105">Dodawanie danych</span><span class="sxs-lookup"><span data-stu-id="11903-105">Adding Data</span></span>

<span data-ttu-id="11903-106">Użyj metody *nieogólnymi. Add* , aby dodać nowe wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="11903-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="11903-107">Dane zostaną wstawione do bazy danych po wywołaniu *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="11903-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="11903-108">Metody dodawania, dołączania i aktualizowania działają na pełnym wykresie obiektów, do których przekazały, zgodnie z opisem w sekcji [powiązane dane](related-data.md) .</span><span class="sxs-lookup"><span data-stu-id="11903-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="11903-109">Alternatywnie Właściwość EntityEntry. State może służyć do ustawiania stanu tylko pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="11903-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="11903-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="11903-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="11903-111">Aktualizowanie danych</span><span class="sxs-lookup"><span data-stu-id="11903-111">Updating Data</span></span>

<span data-ttu-id="11903-112">EF automatycznie wykrywa zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="11903-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="11903-113">Obejmuje to jednostki, które zostały załadowane z bazy danych, oraz obiekty, które zostały wcześniej dodane i zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="11903-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="11903-114">Po prostu Zmodyfikuj wartości przypisane do właściwości, a następnie Wywołaj *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="11903-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="11903-115">Usuwanie danych</span><span class="sxs-lookup"><span data-stu-id="11903-115">Deleting Data</span></span>

<span data-ttu-id="11903-116">Użyj metody *nieogólnymi. Remove* , aby usunąć wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="11903-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="11903-117">Jeśli jednostka już istnieje w bazie danych, zostanie usunięta podczas *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="11903-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="11903-118">Jeśli obiekt nie został jeszcze zapisany w bazie danych (jest śledzony jako dodany), zostanie usunięty z kontekstu i nie będzie już wstawiany, gdy *metody SaveChanges* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="11903-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="11903-119">Wiele operacji w jednym metody SaveChanges</span><span class="sxs-lookup"><span data-stu-id="11903-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="11903-120">Można połączyć wiele operacji dodawania/aktualizowania/usuwania w jedno wywołanie do *metody SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="11903-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="11903-121">W przypadku większości dostawców baz danych *metody SaveChanges* jest transakcyjna.</span><span class="sxs-lookup"><span data-stu-id="11903-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="11903-122">Oznacza to, że wszystkie operacje zakończą się powodzeniem lub niepowodzeniem, a operacje nigdy nie zostaną zastosowane częściowo.</span><span class="sxs-lookup"><span data-stu-id="11903-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
