---
title: Testowanie składników przy użyciu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197493"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="d24f8-102">Testowanie składników przy użyciu EF Core</span><span class="sxs-lookup"><span data-stu-id="d24f8-102">Testing components using EF Core</span></span>

<span data-ttu-id="d24f8-103">Możesz testować składniki przy użyciu czegoś, które przybliżą połączenie z rzeczywistą bazą danych, bez nakładu pracy rzeczywistej operacji we/wy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d24f8-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="d24f8-104">Dostępne są dwie główne opcje:</span><span class="sxs-lookup"><span data-stu-id="d24f8-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="d24f8-105">[Tryb in-memory programu SQLite](sqlite.md) umożliwia pisanie wydajnych testów względem dostawcy, który zachowuje się jak relacyjna baza danych.</span><span class="sxs-lookup"><span data-stu-id="d24f8-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="d24f8-106">[Dostawca inMemory](in-memory.md) jest lekkim dostawcą z minimalnymi zależnościami, ale nie zawsze zachowuje się jak relacyjna baza danych.</span><span class="sxs-lookup"><span data-stu-id="d24f8-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
