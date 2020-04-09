---
title: Zapis podstawowy — ef core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417636"
---
# <a name="basic-save"></a><span data-ttu-id="f7619-102">Zapisywanie podstawowe</span><span class="sxs-lookup"><span data-stu-id="f7619-102">Basic Save</span></span>

<span data-ttu-id="f7619-103">Dowiedz się, jak dodawać, modyfikować i usuwać dane przy użyciu klas kontekstu i encji.</span><span class="sxs-lookup"><span data-stu-id="f7619-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="f7619-104">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="f7619-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="f7619-105">Dodawanie danych</span><span class="sxs-lookup"><span data-stu-id="f7619-105">Adding Data</span></span>

<span data-ttu-id="f7619-106">Użyj *DbSet.Add* metody, aby dodać nowe wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="f7619-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="f7619-107">Dane zostaną wstawione do bazy danych podczas wywoływania *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f7619-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="f7619-108">Metody Dodaj, Dołącz i Aktualizuj wszystkie działają na pełnym wykresie jednostek przekazanych do nich, zgodnie z opisem w sekcji [Dane pokrewne.](related-data.md)</span><span class="sxs-lookup"><span data-stu-id="f7619-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="f7619-109">Alternatywnie EntityEntry.State właściwość może służyć do ustawiania stanu tylko jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7619-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="f7619-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="f7619-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="f7619-111">Aktualizowanie danych</span><span class="sxs-lookup"><span data-stu-id="f7619-111">Updating Data</span></span>

<span data-ttu-id="f7619-112">EF automatycznie wykryje zmiany wprowadzone do istniejącej jednostki, która jest śledzona przez kontekst.</span><span class="sxs-lookup"><span data-stu-id="f7619-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="f7619-113">Obejmuje to jednostki, które można załadować/kwerendy z bazy danych i jednostek, które zostały wcześniej dodane i zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f7619-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="f7619-114">Wystarczy zmodyfikować wartości przypisane do właściwości, a następnie *wywołać SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f7619-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="f7619-115">Usuwanie danych</span><span class="sxs-lookup"><span data-stu-id="f7619-115">Deleting Data</span></span>

<span data-ttu-id="f7619-116">Użyj *DbSet.Remove* metody, aby usunąć wystąpienia klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="f7619-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="f7619-117">Jeśli jednostka już istnieje w bazie danych, zostanie usunięta podczas *savechanges*.</span><span class="sxs-lookup"><span data-stu-id="f7619-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="f7619-118">Jeśli jednostka nie została jeszcze zapisana w bazie danych (oznacza to, że jest śledzona jako dodana), zostanie ona usunięta z kontekstu i nie będzie już wstawiana po *wywołaniu SaveChanges.*</span><span class="sxs-lookup"><span data-stu-id="f7619-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="f7619-119">Wiele operacji w jednym savechanges</span><span class="sxs-lookup"><span data-stu-id="f7619-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="f7619-120">Można połączyć wiele operacji Dodaj/Aktualizuj/Usuń w jedno wywołanie *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="f7619-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="f7619-121">Dla większości dostawców bazy danych *SaveChanges* jest transakcyjnych.</span><span class="sxs-lookup"><span data-stu-id="f7619-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="f7619-122">Oznacza to, że wszystkie operacje zakończy się powodzeniem lub niepowodzeniem, a operacje nigdy nie zostaną częściowo zastosowane.</span><span class="sxs-lookup"><span data-stu-id="f7619-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
