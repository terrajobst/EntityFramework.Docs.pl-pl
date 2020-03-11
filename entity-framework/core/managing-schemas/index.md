---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416306"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="eca7c-102">Zarządzanie schematami bazy danych</span><span class="sxs-lookup"><span data-stu-id="eca7c-102">Managing Database Schemas</span></span>

<span data-ttu-id="eca7c-103">EF Core oferuje dwa podstawowe sposoby utrzymywania synchronizacji modelu EF Core i schematu bazy danych. Aby wybrać jeden z tych dwóch, zdecyduj, czy model EF Core lub schemat bazy danych jest źródłem prawdy.</span><span class="sxs-lookup"><span data-stu-id="eca7c-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="eca7c-104">Jeśli chcesz, aby model EF Core był źródłem prawdy, użyj [migracji][1].</span><span class="sxs-lookup"><span data-stu-id="eca7c-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="eca7c-105">W miarę wprowadzania zmian w modelu EF Core ta metoda stopniowo stosuje odpowiednie zmiany schematu do bazy danych, tak aby była zgodna z modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="eca7c-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="eca7c-106">Jeśli chcesz, aby schemat bazy danych był źródłem prawdy, [Użyj odtwarzania.][2]</span><span class="sxs-lookup"><span data-stu-id="eca7c-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="eca7c-107">Takie podejście umożliwia tworzenie szkieletu DbContext i klas typu jednostki przez odtwarzanie schematu bazy danych do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="eca7c-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="eca7c-108">[Interfejsy API tworzenia i upuszczania][3] mogą również tworzyć schemat bazy danych na podstawie modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="eca7c-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="eca7c-109">Jednak są one głównie do testowania, tworzenia prototypów i innych scenariuszy, w których porzucanie bazy danych jest dopuszczalne.</span><span class="sxs-lookup"><span data-stu-id="eca7c-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
