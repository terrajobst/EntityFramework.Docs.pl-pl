---
title: "Zarządzanie schematów bazy danych — podstawowe EF"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a><span data-ttu-id="6bd63-102">Zarządzanie schematów bazy danych</span><span class="sxs-lookup"><span data-stu-id="6bd63-102">Managing Database Schemas</span></span>
<span data-ttu-id="6bd63-103">Podstawowe EF udostępnia dwa sposoby podstawowego elementu synchronizacja EF Core schematu modelu i bazy danych. Aby wybrać między nimi, należy zdecydować, czy model EF Core lub schemat bazy danych jest źródło prawdy.</span><span class="sxs-lookup"><span data-stu-id="6bd63-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="6bd63-104">Model EF Core były źródłem prawdy, należy użyć [migracje][1].</span><span class="sxs-lookup"><span data-stu-id="6bd63-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="6bd63-105">Po wprowadzeniu dowolnych zmian do modelu EF Core takie podejście przyrostowo dotyczy odpowiedniej zmiany schematu bazy danych tak, aby pozostaje zgodne z modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="6bd63-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="6bd63-106">Użyj [Reverse Engineering] [ 2] Jeśli chcesz, aby były źródłem prawdy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6bd63-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="6bd63-107">Takie podejście umożliwia tworzenie szkieletu obiektu DbContext i klas jednostek typu przez odtwarzanie schemat bazy danych do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6bd63-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="6bd63-108">[Tworzenie i upuść interfejsów API] [ 3] można również utworzyć schemat bazy danych z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6bd63-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="6bd63-109">Są to jednak głównie do testowania, prototypowania i inne scenariusze, gdzie jest dopuszczalne porzucenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6bd63-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
