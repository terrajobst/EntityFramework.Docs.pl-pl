---
title: Testowanie składników używający narzędzia Entity Framework - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997840"
---
# <a name="testing"></a><span data-ttu-id="528e4-102">Testowanie</span><span class="sxs-lookup"><span data-stu-id="528e4-102">Testing</span></span>

<span data-ttu-id="528e4-103">Można przetestować składników za pomocą coś, co przybliża z bazą danych rzeczywistych, bez konieczności operacji We/Wy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="528e4-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="528e4-104">Istnieją dwie główne opcje w ten sposób:</span><span class="sxs-lookup"><span data-stu-id="528e4-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="528e4-105">[Tryb w pamięci SQLite](sqlite.md) pozwala na zapis efektywne testy względem dostawcy, który zachowuje się jak relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="528e4-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="528e4-106">[Dostawca InMemory](in-memory.md) jest uproszczone dostawcę, który ma minimalne zależności, ale nie zawsze zachowują się jak relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="528e4-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
