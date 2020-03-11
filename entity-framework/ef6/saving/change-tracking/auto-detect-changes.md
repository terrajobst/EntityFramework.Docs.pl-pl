---
title: Automatyczne wykrywanie zmian — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416978"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="3082c-102">Automatyczne wykrywanie zmian</span><span class="sxs-lookup"><span data-stu-id="3082c-102">Automatic detect changes</span></span>
<span data-ttu-id="3082c-103">W przypadku korzystania z większości jednostek POCO określenie sposobu zmiany jednostki (i w związku z tym, które aktualizacje muszą zostać wysłane do bazy danych) jest obsługiwane przez algorytm wykrywania zmian.</span><span class="sxs-lookup"><span data-stu-id="3082c-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="3082c-104">Wykrywanie zmian działa przez wykrywanie różnic między bieżącymi wartościami właściwości jednostki i oryginalnymi wartościami właściwości, które są przechowywane w migawce podczas wykonywania zapytania lub dołączania jednostki.</span><span class="sxs-lookup"><span data-stu-id="3082c-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="3082c-105">Techniki przedstawione w tym temacie dotyczą również modeli utworzonych przy użyciu Code First i programu Dr Designer.</span><span class="sxs-lookup"><span data-stu-id="3082c-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="3082c-106">Domyślnie Entity Framework automatycznie wykrywa zmiany w przypadku wywołania następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3082c-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="3082c-107">Nieogólnymi. Find</span><span class="sxs-lookup"><span data-stu-id="3082c-107">DbSet.Find</span></span>  
- <span data-ttu-id="3082c-108">Nieogólnymi. Local</span><span class="sxs-lookup"><span data-stu-id="3082c-108">DbSet.Local</span></span>  
- <span data-ttu-id="3082c-109">Nieogólnymi. Add</span><span class="sxs-lookup"><span data-stu-id="3082c-109">DbSet.Add</span></span>  
- <span data-ttu-id="3082c-110">Nieogólnymi. AddRange</span><span class="sxs-lookup"><span data-stu-id="3082c-110">DbSet.AddRange</span></span>
- <span data-ttu-id="3082c-111">Nieogólnymi. Remove</span><span class="sxs-lookup"><span data-stu-id="3082c-111">DbSet.Remove</span></span>  
- <span data-ttu-id="3082c-112">Nieogólnymi. RemoveRange</span><span class="sxs-lookup"><span data-stu-id="3082c-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="3082c-113">Nieogólnymi. Attach</span><span class="sxs-lookup"><span data-stu-id="3082c-113">DbSet.Attach</span></span>  
- <span data-ttu-id="3082c-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="3082c-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="3082c-115">DbContext. GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="3082c-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="3082c-116">DbContext. entry</span><span class="sxs-lookup"><span data-stu-id="3082c-116">DbContext.Entry</span></span>  
- <span data-ttu-id="3082c-117">DbChangeTracker. wpisy</span><span class="sxs-lookup"><span data-stu-id="3082c-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="3082c-118">Wyłączanie automatycznego wykrywania zmian</span><span class="sxs-lookup"><span data-stu-id="3082c-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="3082c-119">W przypadku śledzenia dużej liczby jednostek w kontekście i wywołania jednej z tych metod wiele razy w pętli, można uzyskać znaczące ulepszenia wydajności, wyłączając wykrywanie zmian na czas trwania pętli.</span><span class="sxs-lookup"><span data-stu-id="3082c-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="3082c-120">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3082c-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

<span data-ttu-id="3082c-121">Nie zapomnij ponownie włączyć wykrywania zmian po pętli — użyto instrukcji try/finally, aby upewnić się, że jest ona zawsze ponownie włączona, nawet jeśli kod w pętli zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="3082c-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="3082c-122">Alternatywą dla wyłączenia i ponownego włączenia jest pozostawienie automatycznego wykrycia, że zmiany są wyłączane przez cały czas i w kontekście wywołania. ChangeTracker. DetectChanges jawnie lub użyj serwerów proxy śledzenia zmian pracujemy.</span><span class="sxs-lookup"><span data-stu-id="3082c-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="3082c-123">Obie te opcje są zaawansowane i mogą łatwo wprowadzać drobne błędy do aplikacji, dzięki czemu są one używane z opieką.</span><span class="sxs-lookup"><span data-stu-id="3082c-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="3082c-124">Jeśli konieczne jest dodanie lub usunięcie wielu obiektów z kontekstu, należy rozważyć użycie Nieogólnymi. AddRange i Nieogólnymi. RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="3082c-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="3082c-125">Te metody automatycznie wykrywają zmiany tylko raz po zakończeniu operacji dodawania lub usuwania.</span><span class="sxs-lookup"><span data-stu-id="3082c-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
