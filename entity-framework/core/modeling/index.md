---
title: Tworzenie modelu - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a>Tworzenie modelu

Entity Framework korzysta z zestawu Konwencji do kompilacji jest model oparty na kształt z klas jednostek. Można określić dodatkowej konfiguracji do uzupełnienia i/lub zastąpienie co została wykryta przez Konwencję.

W tym artykule omówiono konfiguracji, który można zastosować do modelu przeznaczonych dla dowolnego magazynu danych i tych, które mogą być stosowane, gdy wszystkie relacyjnej bazy danych. Dostawcy mogą również włączyć konfiguracji, które są specyficzne dla magazynu danych. Dokumentację dotyczącą określonej konfiguracji dostawcy znaleźć [dostawcy bazy danych](../providers/index.md) sekcji.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) w witrynie GitHub.

## <a name="methods-of-configuration"></a>Metody konfiguracji

### <a name="fluent-api"></a>Interfejsu API Fluent

Można zastąpić `OnModelCreating` metodę w pochodnej kontekstu i użyj `ModelBuilder API` do skonfigurowania modelu. To jest najbardziej zaawansowanych metody konfiguracji i umożliwia konfigurację można określić bez modyfikowania klas jednostek. Konfiguracja interfejsu API Fluent ma najwyższy priorytet i spowoduje zastąpienie adnotacje konwencje i danych.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a>Adnotacji danych

Można również zastosować atrybutów (nazywane adnotacji danych) do klasy i właściwości. Adnotacje danych spowoduje zastąpienie Konwencji, ale zostaną zastąpione przez konfigurację interfejsu API Fluent.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
