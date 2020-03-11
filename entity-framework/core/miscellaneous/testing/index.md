---
title: Testowanie składników przy użyciu EF Core-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416562"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="41aa8-102">Testowanie składników przy użyciu EF Core</span><span class="sxs-lookup"><span data-stu-id="41aa8-102">Testing components using EF Core</span></span>

<span data-ttu-id="41aa8-103">Możesz testować składniki przy użyciu czegoś, które przybliżą połączenie z rzeczywistą bazą danych, bez nakładu pracy rzeczywistej operacji we/wy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="41aa8-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="41aa8-104">Dostępne są dwie główne opcje:</span><span class="sxs-lookup"><span data-stu-id="41aa8-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="41aa8-105">[Tryb in-memory programu SQLite](sqlite.md) umożliwia pisanie wydajnych testów względem dostawcy, który zachowuje się jak relacyjna baza danych.</span><span class="sxs-lookup"><span data-stu-id="41aa8-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="41aa8-106">[Dostawca inMemory](in-memory.md) jest lekkim dostawcą z minimalnymi zależnościami, ale nie zawsze zachowuje się jak relacyjna baza danych.</span><span class="sxs-lookup"><span data-stu-id="41aa8-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
