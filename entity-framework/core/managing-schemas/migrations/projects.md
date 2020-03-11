---
title: Korzystanie z oddzielnego projektu migracji — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416816"
---
# <a name="using-a-separate-migrations-project"></a>Korzystanie z oddzielnego projektu migracji

Możesz chcieć przechowywać migracje w innym zestawie niż ten, który zawiera `DbContext`. Ta strategia służy również do obsługi wielu zestawów migracji, na przykład jednej do programowania i dla uaktualnień Release-to-Release.

Aby wykonać tę czynność...

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

## <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
