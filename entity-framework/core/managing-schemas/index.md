---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994388"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="a90ae-102">Zarządzanie schematami bazy danych</span><span class="sxs-lookup"><span data-stu-id="a90ae-102">Managing Database Schemas</span></span>
<span data-ttu-id="a90ae-103">EF Core zawiera dwa podstawowe sposoby synchronizacja schematu modelu i bazie danych EF Core. Aby wybrać między nimi, należy zdecydować, czy modelu platformy EF Core lub schemat bazy danych jest źródło prawdziwych danych.</span><span class="sxs-lookup"><span data-stu-id="a90ae-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="a90ae-104">Aby modelu platformy EF Core jako źródło prawdziwych danych, należy użyć [migracje][1].</span><span class="sxs-lookup"><span data-stu-id="a90ae-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="a90ae-105">Po wprowadzeniu dowolnych zmian do modelu platformy EF Core, to podejście przyrostowo dotyczy odpowiedniej zmiany schematu bazy danych, aby pozostaje zgodny z modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="a90ae-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="a90ae-106">Użyj [odwrotnego Engineering] [ 2] Jeśli schemat bazy danych jako źródło prawdziwych danych.</span><span class="sxs-lookup"><span data-stu-id="a90ae-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="a90ae-107">Takie podejście umożliwia tworzenia szkieletu typu DbContext i klasami typu jednostki przez odtwarzanie schemat bazy danych do modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="a90ae-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="a90ae-108">[Tworzenie i upuszczanie interfejsów API] [ 3] można również utworzyć schemat bazy danych z modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="a90ae-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="a90ae-109">Jednak są one głównie do testowania, tworzenia prototypów i innych scenariuszy, w których porzucenie bazy danych jest do zaakceptowania.</span><span class="sxs-lookup"><span data-stu-id="a90ae-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
