---
title: Migracje z wieloma projektami - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
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
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
