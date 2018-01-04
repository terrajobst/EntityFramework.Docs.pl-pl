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
---
<a name="using-a-separate-project"></a>Przy użyciu osobnych projektu
========================
Warto przechowywać migracji w innym zestawie niż jeden zawierający Twoje `DbContext`. Do uaktualnienia wersji do wersji, można użyć tej strategii do obsługi wielu zestawów migracji, na przykład, jeden dla rozwoju i drugiego.

Aby to zrobić...

1. Tworzenie nowej biblioteki klas.

2. Dodaj odwołanie do zestawu z typu DbContext.

3. Przenieś migracji i modelu migawki plików do biblioteki klas.
   * Jeśli nie dodano żadnych serwerów, dodaj je do projektu typu DbContext, a następnie go przenieść.

4. Skonfiguruj zestaw migracji:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Dodaj odwołanie do zestawu z migracji z zestawu uruchamiania.
   * Jeśli spowoduje utworzenie zależności cyklicznej, należy zaktualizować ścieżki wyjściowej biblioteki klas:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Jeśli tak, nie wszystkie poprawnie, należy dodać nowy migracji do projektu.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
