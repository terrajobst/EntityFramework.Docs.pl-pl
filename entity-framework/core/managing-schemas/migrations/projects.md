---
title: Migracje z wieloma projektami - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161230"
---
<a name="using-a-separate-project"></a><span data-ttu-id="abfb9-102">Przy użyciu osobnych projektu</span><span class="sxs-lookup"><span data-stu-id="abfb9-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="abfb9-103">Warto przechowywać migracji w innym zestawie niż jeden zawierający Twoje `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="abfb9-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="abfb9-104">Do uaktualnienia wersji do wersji, można użyć tej strategii do obsługi wielu zestawów migracji, na przykład, jeden dla rozwoju i drugiego.</span><span class="sxs-lookup"><span data-stu-id="abfb9-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="abfb9-105">Aby to zrobić...</span><span class="sxs-lookup"><span data-stu-id="abfb9-105">To do this...</span></span>

1. <span data-ttu-id="abfb9-106">Tworzenie nowej biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="abfb9-106">Create a new class library.</span></span>

2. <span data-ttu-id="abfb9-107">Dodaj odwołanie do zestawu z typu DbContext.</span><span class="sxs-lookup"><span data-stu-id="abfb9-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="abfb9-108">Przenieś migracji i modelu migawki plików do biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="abfb9-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="abfb9-109">Jeśli nie dodano żadnych serwerów, dodaj je do projektu typu DbContext, a następnie go przenieść.</span><span class="sxs-lookup"><span data-stu-id="abfb9-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="abfb9-110">Skonfiguruj zestaw migracji:</span><span class="sxs-lookup"><span data-stu-id="abfb9-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="abfb9-111">Dodaj odwołanie do zestawu z migracji z zestawu uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="abfb9-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="abfb9-112">Jeśli spowoduje utworzenie zależności cyklicznej, należy zaktualizować ścieżki wyjściowej biblioteki klas:</span><span class="sxs-lookup"><span data-stu-id="abfb9-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="abfb9-113">Jeśli tak, nie wszystkie poprawnie, należy dodać nowy migracji do projektu.</span><span class="sxs-lookup"><span data-stu-id="abfb9-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
