---
title: Migracje w środowiskach zespołów — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655544"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="4a5f3-102">Migracje w środowiskach zespołów</span><span class="sxs-lookup"><span data-stu-id="4a5f3-102">Migrations in Team Environments</span></span>

<span data-ttu-id="4a5f3-103">Podczas pracy z migracjami w środowiskach zespołu należy zwrócić szczególną uwagę na plik migawek modelu.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="4a5f3-104">Ten plik może poinformować użytkownika, jeśli migracja Twojego zespołu nie zostanie prawidłowo scalona z Twoimi lub jeśli trzeba rozwiązać konflikt przez ponowne utworzenie migracji przed udostępnieniem jej.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="4a5f3-105">Scalanie</span><span class="sxs-lookup"><span data-stu-id="4a5f3-105">Merging</span></span>

<span data-ttu-id="4a5f3-106">Po scaleniu migracji od członków zespołu mogą wystąpić konflikty w pliku migawek modelu.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="4a5f3-107">Jeśli obie zmiany są niepowiązane, scalanie jest proste, a dwie migracje mogą współistnieć.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="4a5f3-108">Na przykład w konfiguracji typu jednostki klienta może wystąpić konflikt scalania, który wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="4a5f3-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

<span data-ttu-id="4a5f3-109">Ponieważ obie te właściwości muszą istnieć w modelu końcowym, Ukończ scalanie przez dodanie obu właściwości.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="4a5f3-110">W wielu przypadkach system kontroli wersji może automatycznie scalić takie zmiany.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="4a5f3-111">W takich przypadkach migracja i migracja Twojego zespołu są niezależne od siebie.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="4a5f3-112">Ponieważ jedna z nich może zostać zastosowana jako pierwsza, nie musisz wprowadzać żadnych dodatkowych zmian do migracji przed udostępnieniem jej zespołowi.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="4a5f3-113">Rozwiązywanie konfliktów</span><span class="sxs-lookup"><span data-stu-id="4a5f3-113">Resolving conflicts</span></span>

<span data-ttu-id="4a5f3-114">Czasami występuje konflikt w czasie scalania modelu migawek modelu.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="4a5f3-115">Na przykład ty i Twojemu członkowi zespołu można zmienić nazwę tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-115">For example, you and your teammate may each have renamed the same property.</span></span>

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

<span data-ttu-id="4a5f3-116">Jeśli wystąpi ten rodzaj konfliktu, rozwiąż go przez ponowne utworzenie migracji.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="4a5f3-117">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="4a5f3-117">Follow these steps:</span></span>

1. <span data-ttu-id="4a5f3-118">Przerwij scalanie i wycofywanie do katalogu roboczego przed scaleniem</span><span class="sxs-lookup"><span data-stu-id="4a5f3-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="4a5f3-119">Usuń migrację (ale Zachowaj zmiany modelu)</span><span class="sxs-lookup"><span data-stu-id="4a5f3-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="4a5f3-120">Scalanie zmian Twojego zespołu w katalogu roboczym</span><span class="sxs-lookup"><span data-stu-id="4a5f3-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="4a5f3-121">Ponowne Dodawanie migracji</span><span class="sxs-lookup"><span data-stu-id="4a5f3-121">Re-add your migration</span></span>

<span data-ttu-id="4a5f3-122">Po wykonaniu tej czynności te dwie migracje mogą być stosowane w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="4a5f3-123">Najpierw zostanie zastosowana migracja, zmiana nazwy kolumny na *alias*, a następnie migracja zmienia nazwę na *nazwa_użytkownika*.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="4a5f3-124">Migracja może być bezpiecznie udostępniona w pozostałej części zespołu.</span><span class="sxs-lookup"><span data-stu-id="4a5f3-124">Your migration can safely be shared with the rest of the team.</span></span>
