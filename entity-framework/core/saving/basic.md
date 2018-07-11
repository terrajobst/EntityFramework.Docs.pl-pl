---
title: Zapisywanie podstawowe - programu EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: ecf8f344a5baae37a5e7255a4affb1085f1b3ff3
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949403"
---
# <a name="basic-save"></a><span data-ttu-id="be587-102">Zapisywanie podstawowe</span><span class="sxs-lookup"><span data-stu-id="be587-102">Basic Save</span></span>

<span data-ttu-id="be587-103">Dowiedz się, jak dodawanie, modyfikowanie i usuwanie danych przy użyciu klas jednostek i kontekstu.</span><span class="sxs-lookup"><span data-stu-id="be587-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="be587-104">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="be587-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="be587-105">Dodawanie danych</span><span class="sxs-lookup"><span data-stu-id="be587-105">Adding Data</span></span>

<span data-ttu-id="be587-106">Użyj *DbSet.Add* metodę, aby dodać nowe wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="be587-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="be587-107">Dane zostanie wstawiony w bazie danych podczas wywoływania *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="be587-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="be587-108">Metody Add, Dołącz i aktualizacji, wszystkie robocze na pełny wykres jednostek jest przekazywany do nich, zgodnie z opisem w [powiązanych danych](related-data.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="be587-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="be587-109">Alternatywnie właściwość EntityEntry.State może służyć do ustawiania stanu pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="be587-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="be587-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="be587-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="be587-111">Aktualizowanie danych</span><span class="sxs-lookup"><span data-stu-id="be587-111">Updating Data</span></span>

<span data-ttu-id="be587-112">EF automatycznie wykrywa zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="be587-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="be587-113">Obejmuje to jednostki, które możesz obciążenia/zapytań z bazy danych i jednostek, które zostały wcześniej dodane i zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="be587-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="be587-114">Po prostu zmodyfikuj wartości przypisane do właściwości, a następnie wywołać *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="be587-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="be587-115">Usuwanie danych</span><span class="sxs-lookup"><span data-stu-id="be587-115">Deleting Data</span></span>

<span data-ttu-id="be587-116">Użyj *DbSet.Remove* metody, można usunąć wystąpień klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="be587-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="be587-117">Jeśli ta jednostka już istnieje w bazie danych, zostaną usunięte podczas *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="be587-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="be587-118">Jeśli jednostka nie został jeszcze zapisany w bazie danych (czyli jego śledzenia dodawania) zostaną usunięte z kontekstu, a nie będzie już wstawiany gdy *SaveChanges* jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="be587-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="be587-119">Wiele operacji w jednym SaveChanges</span><span class="sxs-lookup"><span data-stu-id="be587-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="be587-120">Można połączyć wiele operacji Dodawanie/aktualizowanie/usuwanie na jedno wywołanie *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="be587-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="be587-121">W przypadku większości dostawców bazy danych *SaveChanges* jest transakcyjna.</span><span class="sxs-lookup"><span data-stu-id="be587-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="be587-122">Oznacza to wszystkie operacje będą sukcesem lub niepowodzeniem, a operacje nigdy nie lewej częściowo zastosowanych.</span><span class="sxs-lookup"><span data-stu-id="be587-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
