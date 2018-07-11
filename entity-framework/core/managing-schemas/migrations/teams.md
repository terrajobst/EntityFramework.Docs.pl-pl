---
title: Migracja w środowiskach zespołu — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228383"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="ee866-102">Migracja w środowiskach zespołu</span><span class="sxs-lookup"><span data-stu-id="ee866-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="ee866-103">Podczas pracy z migracji w środowiskach zespołu, należy zwracać szczególną uwagę na migawki pliku modelu.</span><span class="sxs-lookup"><span data-stu-id="ee866-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="ee866-104">Ten plik może określić, czy migracja z partnerem nie pozostawia żadnych śladów scala z Twoimi czy trzeba rozwiązać konflikt, ponownie tworząc migracji przed ich udostępnieniem.</span><span class="sxs-lookup"><span data-stu-id="ee866-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="ee866-105">Scalanie</span><span class="sxs-lookup"><span data-stu-id="ee866-105">Merging</span></span>
-------
<span data-ttu-id="ee866-106">Podczas migracji scalania z członkami zespołu, może wystąpić konflikty w modelu migawki pliku.</span><span class="sxs-lookup"><span data-stu-id="ee866-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="ee866-107">W przypadku niepowiązanych obie zmiany scalanie jest proste i dwie migracje mogą współistnieć.</span><span class="sxs-lookup"><span data-stu-id="ee866-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="ee866-108">Na przykład może wystąpić konflikt scalania w konfiguracji typu jednostki Klient, który wygląda tak:</span><span class="sxs-lookup"><span data-stu-id="ee866-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="ee866-109">Ponieważ obu tych właściwości muszą istnieć w ostatnim modelu Ukończ scalanie, dodając obie te właściwości.</span><span class="sxs-lookup"><span data-stu-id="ee866-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="ee866-110">W wielu przypadkach system kontroli wersji może automatycznie scalić takich zmian.</span><span class="sxs-lookup"><span data-stu-id="ee866-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="ee866-111">W takich przypadkach migracji i migrację z partnerem są niezależne od siebie nawzajem.</span><span class="sxs-lookup"><span data-stu-id="ee866-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="ee866-112">Ponieważ którąś z tych funkcji można najpierw zastosować, nie potrzebujesz dodatkowych zmian przed ich udostępnieniem ze swoim zespołem migracji.</span><span class="sxs-lookup"><span data-stu-id="ee866-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="ee866-113">Rozwiązywanie konfliktów</span><span class="sxs-lookup"><span data-stu-id="ee866-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="ee866-114">Czasami wystąpią true konflikt podczas scalania model modelu migawki.</span><span class="sxs-lookup"><span data-stu-id="ee866-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="ee866-115">Na przykład możesz i z partnerem każdego zmieniona tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="ee866-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="ee866-116">Jeśli wystąpi tego rodzaju konflikt rozwiązać ten problem, ponownie utworzyć plan migracji.</span><span class="sxs-lookup"><span data-stu-id="ee866-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="ee866-117">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="ee866-117">Follow these steps:</span></span>

1. <span data-ttu-id="ee866-118">Przerwij, scalania i wycofania do katalogu roboczego przed scaleniem</span><span class="sxs-lookup"><span data-stu-id="ee866-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="ee866-119">Usuń plan migracji (ale zachować zmiany modelu)</span><span class="sxs-lookup"><span data-stu-id="ee866-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="ee866-120">Scal zmiany z partnerem w katalogu roboczym</span><span class="sxs-lookup"><span data-stu-id="ee866-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="ee866-121">Ponowne dodanie migracji</span><span class="sxs-lookup"><span data-stu-id="ee866-121">Re-add your migration</span></span>

<span data-ttu-id="ee866-122">Po wykonaniu tego, dwie migracje mogą być stosowane w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ee866-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="ee866-123">Ich migracji zostanie zastosowana jako pierwsza, zmiana nazwy kolumny, która ma *Alias*, po tej dacie migracji zmienia jego nazwę, aby *Username*.</span><span class="sxs-lookup"><span data-stu-id="ee866-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="ee866-124">Migracja może być bezpiecznie udostępniane reszta zespołu.</span><span class="sxs-lookup"><span data-stu-id="ee866-124">Your migration can safely be shared with the rest of the team.</span></span>
