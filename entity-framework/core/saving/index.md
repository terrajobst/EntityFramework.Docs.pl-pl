---
title: Zapisywanie danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417549"
---
# <a name="saving-data"></a><span data-ttu-id="f0783-102">Zapisywanie danych</span><span class="sxs-lookup"><span data-stu-id="f0783-102">Saving Data</span></span>

<span data-ttu-id="f0783-103">Każde wystąpienie kontekstu ma `ChangeTracker`, które jest odpowiedzialne za śledzenie zmian, które muszą być zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f0783-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="f0783-104">Podczas wprowadzania zmian w wystąpieniach klas jednostek te zmiany są rejestrowane w `ChangeTracker` a następnie zapisywane w bazie danych podczas wywoływania `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="f0783-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="f0783-105">Dostawca bazy danych jest odpowiedzialny za tłumaczenie zmian na operacje specyficzne dla bazy danych (na przykład `INSERT`, `UPDATE`i `DELETE` poleceń dla relacyjnej bazy danych).</span><span class="sxs-lookup"><span data-stu-id="f0783-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
