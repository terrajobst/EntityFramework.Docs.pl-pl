---
title: Migracje z wieloma projektami — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447147"
---
<a name="using-a-separate-project"></a><span data-ttu-id="9b4e1-102">Korzystanie z osobnego projektu</span><span class="sxs-lookup"><span data-stu-id="9b4e1-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="9b4e1-103">Możesz chcieć przechowywać migracji w innym zestawie niż jeden zawierający swoje `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="9b4e1-104">Do uaktualnienia wersji do wersji, można użyć tej strategii do obsługi wielu zestawów migracji, na przykład, jeden do tworzenia aplikacji i inny.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="9b4e1-105">Aby to zrobić...</span><span class="sxs-lookup"><span data-stu-id="9b4e1-105">To do this...</span></span>

1. <span data-ttu-id="9b4e1-106">Utwórz nową bibliotekę klas.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-106">Create a new class library.</span></span>

2. <span data-ttu-id="9b4e1-107">Dodaj odwołanie do zestawu DbContext.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="9b4e1-108">Przenieś migracje i pliki migawki modelu do biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="9b4e1-109">Jeśli nie masz żadnych istniejących migracje, wygeneruje w do projektu zawierającego kontekstu DbContext, a następnie przenieś go.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="9b4e1-110">Jest to ważne, ponieważ jeśli zestaw migracje nie zawiera istniejącego migracji, polecenia Add-migracji nie można odnaleźć kontekstu DbContext.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="9b4e1-111">Skonfiguruj zestaw migracji:</span><span class="sxs-lookup"><span data-stu-id="9b4e1-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="9b4e1-112">Dodaj odwołanie do zestawu migracje z zestawu startowego.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="9b4e1-113">Jeśli spowoduje to utworzenie zależności cyklicznej, zaktualizuj ścieżkę wyjściową biblioteki klas:</span><span class="sxs-lookup"><span data-stu-id="9b4e1-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="9b4e1-114">Jeśli tak, nie wszystko prawidłowo, należy dodać nowe migracji do projektu.</span><span class="sxs-lookup"><span data-stu-id="9b4e1-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
