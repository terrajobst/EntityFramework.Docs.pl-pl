---
title: Zapisywanie danych — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949398"
---
# <a name="saving-data"></a><span data-ttu-id="df9b1-102">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="df9b1-102">Saving Data</span></span>

<span data-ttu-id="df9b1-103">Każde wystąpienie kontekstu ma `ChangeTracker` odpowiada do śledzenia zmian, które muszą zostać zapisane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="df9b1-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="df9b1-104">Po wprowadzeniu dowolnych zmian do wystąpień klas jednostek, zmiany te są rejestrowane w `ChangeTracker` i następnie zapisywane w bazie danych podczas wywoływania `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="df9b1-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="df9b1-105">Dostawca bazy danych jest odpowiedzialny za tłumaczenie zmiany do określonej bazy danych operacji (na przykład `INSERT`, `UPDATE`, i `DELETE` polecenia do relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="df9b1-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
