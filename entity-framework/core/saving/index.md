---
title: Zapisywanie danych - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054209"
---
# <a name="saving-data"></a><span data-ttu-id="81e01-102">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="81e01-102">Saving Data</span></span>

<span data-ttu-id="81e01-103">Każde wystąpienie kontekstu ma `ChangeTracker` odpowiada celu śledzenia zmian, które muszą być zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="81e01-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="81e01-104">Po wprowadzeniu dowolnych zmian do wystąpień klas jednostki, te zmiany są zapisywane w `ChangeTracker` , a następnie zapisane w bazie danych podczas wywoływania `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="81e01-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="81e01-105">Dostawca bazy danych jest odpowiedzialny za tłumaczenia zmiany w operacjach bazy danych (np. `INSERT`, `UPDATE`, i `DELETE` polecenia dla relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="81e01-105">The database provider is responsible for translating the changes into database-specific operations (e.g. `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
