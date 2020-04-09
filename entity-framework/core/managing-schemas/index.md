---
title: Zarządzanie schematami bazy danych — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416306"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="07fdf-102">Zarządzanie schematami bazy danych</span><span class="sxs-lookup"><span data-stu-id="07fdf-102">Managing Database Schemas</span></span>

<span data-ttu-id="07fdf-103">EF Core zapewnia dwa podstawowe sposoby przechowywania modelu EF Core i schematu bazy danych w synchronizacji. Aby wybrać między tymi dwoma, zdecydować, czy model EF Core lub schemat bazy danych jest źródłem prawdy.</span><span class="sxs-lookup"><span data-stu-id="07fdf-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="07fdf-104">Jeśli chcesz, aby twój model EF Core był źródłem prawdy, użyj [migrations][1].</span><span class="sxs-lookup"><span data-stu-id="07fdf-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="07fdf-105">W miarę wprowadzania zmian w modelu EF Core, to podejście stopniowo stosuje odpowiednie zmiany schematu do bazy danych, dzięki czemu pozostaje zgodna z modelem EF Core.</span><span class="sxs-lookup"><span data-stu-id="07fdf-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="07fdf-106">Użyj [inżynierii odwrotnej,][2] jeśli chcesz, aby schemat bazy danych był źródłem prawdy.</span><span class="sxs-lookup"><span data-stu-id="07fdf-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="07fdf-107">Takie podejście umożliwia szkieletowanie DbContext i klasy typu jednostki przez inżynierii odwrotnej schematu bazy danych do modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="07fdf-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="07fdf-108">Interfejsy [API tworzenia i upuszczania][3] można również utworzyć schemat bazy danych z modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="07fdf-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="07fdf-109">Jednak są one przede wszystkim do testowania, prototypowania i innych scenariuszy, w których upuszczanie bazy danych jest dopuszczalne.</span><span class="sxs-lookup"><span data-stu-id="07fdf-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
