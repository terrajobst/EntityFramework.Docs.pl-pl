---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655647"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="6ed42-102">Zarządzanie schematami bazy danych</span><span class="sxs-lookup"><span data-stu-id="6ed42-102">Managing Database Schemas</span></span>

<span data-ttu-id="6ed42-103">EF Core oferuje dwa podstawowe sposoby utrzymywania synchronizacji modelu EF Core i schematu bazy danych. Aby wybrać jeden z tych dwóch, zdecyduj, czy model EF Core lub schemat bazy danych jest źródłem prawdy.</span><span class="sxs-lookup"><span data-stu-id="6ed42-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="6ed42-104">Jeśli chcesz, aby model EF Core był źródłem prawdy, użyj [migracji][1].</span><span class="sxs-lookup"><span data-stu-id="6ed42-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="6ed42-105">W miarę wprowadzania zmian w modelu EF Core ta metoda stopniowo stosuje odpowiednie zmiany schematu do bazy danych, tak aby była zgodna z modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="6ed42-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="6ed42-106">Jeśli chcesz, aby schemat bazy danych był źródłem prawdy, [Użyj odtwarzania.][2]</span><span class="sxs-lookup"><span data-stu-id="6ed42-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="6ed42-107">Takie podejście umożliwia tworzenie szkieletu DbContext i klas typu jednostki przez odtwarzanie schematu bazy danych do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6ed42-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="6ed42-108">[Interfejsy API tworzenia i upuszczania][3] mogą również tworzyć schemat bazy danych na podstawie modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6ed42-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="6ed42-109">Jednak są one głównie do testowania, tworzenia prototypów i innych scenariuszy, w których porzucanie bazy danych jest dopuszczalne.</span><span class="sxs-lookup"><span data-stu-id="6ed42-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
