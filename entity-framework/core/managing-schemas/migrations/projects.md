---
title: Korzystanie z oddzielnego projektu migracji — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812031"
---
# <a name="using-a-separate-migrations-project"></a>Korzystanie z oddzielnego projektu migracji

Możesz chcieć przechowywać migracje w innym zestawie niż ten, który zawiera `DbContext`. Ta strategia służy również do obsługi wielu zestawów migracji, na przykład jednej do programowania i dla uaktualnień Release-to-Release.

Aby to zrobić...

1. Utwórz nową bibliotekę klas.

2. Dodaj odwołanie do zestawu DbContext.

3. Przenieś migracje i pliki migawek modelu do biblioteki klas.
   > [!TIP]
   > Jeśli nie masz żadnych istniejących migracji, wygeneruj je w projekcie zawierającym DbContext, a następnie przenieś go.
   > Jest to ważne, ponieważ w przypadku, gdy zestaw migracji nie zawiera istniejącej migracji, polecenie Add-Migration nie będzie mogło odnaleźć kontekstu DB.

4. Skonfiguruj zestaw migracji:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Dodaj odwołanie do zestawu migracji z zestawu startowego.
   * Jeśli spowoduje to zależność cykliczną, zaktualizuj ścieżkę wyjściową biblioteki klas:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Jeśli wszystko zostało prawidłowo wykonane, powinno być możliwe dodanie nowych migracji do projektu.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
