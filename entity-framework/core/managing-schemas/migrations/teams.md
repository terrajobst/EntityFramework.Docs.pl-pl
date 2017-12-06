---
title: "Migracja w środowiskach zespołu - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="dc2a8-102">Migracja w środowiskach zespołu</span><span class="sxs-lookup"><span data-stu-id="dc2a8-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="dc2a8-103">Podczas pracy z migracji w środowiskach zespołu, należy zwrócić uwagę dodatkowe migawki pliku modelu.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="dc2a8-104">Ten plik można stwierdzić, jeśli migracja z partnerem scala prawidłowo z Twoimi zmianami z Jeśli chcesz rozwiązać konflikt, ponownie tworząc migracji przed udostępnieniem go.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="dc2a8-105">Scalanie</span><span class="sxs-lookup"><span data-stu-id="dc2a8-105">Merging</span></span>
-------
<span data-ttu-id="dc2a8-106">Podczas migracji scalania z członków zespołu, mogą wystąpić konflikty w pliku modelu migawki.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="dc2a8-107">W przypadku niepowiązanych zmiany scalania jest proste i dwa migracje mogą współistnieć.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="dc2a8-108">Na przykład może zostać konfliktu scalania w konfiguracji typu jednostki klienta, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="dc2a8-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="dc2a8-109">Ponieważ obu tych właściwości muszą istnieć w ostatnim modelu scalania, dodając obu właściwości.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="dc2a8-110">W wielu przypadkach system kontroli wersji może automatycznie scalić zmian.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="dc2a8-111">W takich przypadkach migracji i migracji z partnerem są od siebie niezależne.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="dc2a8-112">Ponieważ każdej z nich może być zastosowana jako pierwsza, nie trzeba wprowadzać żadnych zmian dodatkowe migracji przed udostępnieniem go z zespołem.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="dc2a8-113">Rozwiązywanie konfliktów</span><span class="sxs-lookup"><span data-stu-id="dc2a8-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="dc2a8-114">Czasami wystąpią true konflikt podczas scalania modelu migawki modelu.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="dc2a8-115">Na przykład możesz i z partnerem każdego zmieniona tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="dc2a8-116">Jeśli wystąpią tego rodzaju konflikt rozwiązanie go ponownie tworząc migracji.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="dc2a8-117">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="dc2a8-117">Follow these steps:</span></span>

1. <span data-ttu-id="dc2a8-118">Przerwij scalania i wycofania do katalogu roboczego przed scaleniem</span><span class="sxs-lookup"><span data-stu-id="dc2a8-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="dc2a8-119">Usuń migracji (ale zachować zmiany modelu)</span><span class="sxs-lookup"><span data-stu-id="dc2a8-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="dc2a8-120">Scal zmiany z partnerem w katalogu roboczego</span><span class="sxs-lookup"><span data-stu-id="dc2a8-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="dc2a8-121">Dodaj ponownie migracji</span><span class="sxs-lookup"><span data-stu-id="dc2a8-121">Re-add your migration</span></span>

<span data-ttu-id="dc2a8-122">Po wykonaniu tej czynności dwa migracje mogą być stosowane w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="dc2a8-123">Ich migracji zostanie zastosowana jako pierwsza, zmiana nazwy kolumny, która ma *Alias*, następnie migracji zmienia jego nazwę, aby *Username*.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="dc2a8-124">Migracji można bezpiecznie udostępniony reszty zespołu.</span><span class="sxs-lookup"><span data-stu-id="dc2a8-124">Your migration can safely be shared with the rest of the team.</span></span>
