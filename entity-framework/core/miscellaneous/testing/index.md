---
title: "Testowanie przy użyciu programu Entity Framework - EF podstawowych składników"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="67b5b-102">Testowanie</span><span class="sxs-lookup"><span data-stu-id="67b5b-102">Testing</span></span>

<span data-ttu-id="67b5b-103">Można przetestować składniki przy użyciu funkcji, która przybliża łączenia z bazą danych rzeczywistych, bez potrzeby rzeczywistej bazy danych operacji We/Wy.</span><span class="sxs-lookup"><span data-stu-id="67b5b-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="67b5b-104">Istnieją dwie główne opcje, aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="67b5b-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="67b5b-105">[Trybu w pamięci SQLite](sqlite.md) umożliwia pisanie wydajne testy względem dostawcy, który zachowuje się jak relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67b5b-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="67b5b-106">[Dostawca InMemory](in-memory.md) jest lekki dostawcy, który ma minimalny zależności, ale nie zawsze zachowywać się jak relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="67b5b-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
