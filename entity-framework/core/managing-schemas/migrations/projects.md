---
title: Migracje z wieloma projektami — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997990"
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
