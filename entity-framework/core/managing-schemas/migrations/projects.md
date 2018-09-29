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
<a name="using-a-separate-project"></a>Korzystanie z osobnego projektu
========================
Możesz chcieć przechowywać migracji w innym zestawie niż jeden zawierający swoje `DbContext`. Do uaktualnienia wersji do wersji, można użyć tej strategii do obsługi wielu zestawów migracji, na przykład, jeden do tworzenia aplikacji i inny.

Aby to zrobić...

1. Utwórz nową bibliotekę klas.

2. Dodaj odwołanie do zestawu DbContext.

3. Przenieś migracje i pliki migawki modelu do biblioteki klas.
   > [!TIP]
   > Jeśli nie masz żadnych istniejących migracje, wygeneruje w do projektu zawierającego kontekstu DbContext, a następnie przenieś go. Jest to ważne, ponieważ jeśli zestaw migracje nie zawiera istniejącego migracji, polecenia Add-migracji nie można odnaleźć kontekstu DbContext.

4. Skonfiguruj zestaw migracji:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Dodaj odwołanie do zestawu migracje z zestawu startowego.
   * Jeśli spowoduje to utworzenie zależności cyklicznej, zaktualizuj ścieżkę wyjściową biblioteki klas:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Jeśli tak, nie wszystko prawidłowo, należy dodać nowe migracji do projektu.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
