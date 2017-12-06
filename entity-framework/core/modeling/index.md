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
# <a name="creating-a-model"></a><span data-ttu-id="c2a77-102">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="c2a77-102">Creating a Model</span></span>

<span data-ttu-id="c2a77-103">Entity Framework korzysta z zestawu Konwencji do kompilacji jest model oparty na kształt z klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="c2a77-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="c2a77-104">Można określić dodatkowej konfiguracji do uzupełnienia i/lub zastąpienie co została wykryta przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="c2a77-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="c2a77-105">W tym artykule omówiono konfiguracji, który można zastosować do modelu przeznaczonych dla dowolnego magazynu danych i tych, które mogą być stosowane, gdy wszystkie relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c2a77-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="c2a77-106">Dostawcy mogą również włączyć konfiguracji, które są specyficzne dla magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="c2a77-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="c2a77-107">Dokumentację dotyczącą określonej konfiguracji dostawcy znaleźć [dostawcy bazy danych](../providers/index.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c2a77-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="c2a77-108">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="c2a77-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="c2a77-109">Metody konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c2a77-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="c2a77-110">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="c2a77-110">Fluent API</span></span>

<span data-ttu-id="c2a77-111">Można zastąpić `OnModelCreating` metodę w pochodnej kontekstu i użyj `ModelBuilder API` do skonfigurowania modelu.</span><span class="sxs-lookup"><span data-stu-id="c2a77-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="c2a77-112">To jest najbardziej zaawansowanych metody konfiguracji i umożliwia konfigurację można określić bez modyfikowania klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="c2a77-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="c2a77-113">Konfiguracja interfejsu API Fluent ma najwyższy priorytet i spowoduje zastąpienie adnotacje konwencje i danych.</span><span class="sxs-lookup"><span data-stu-id="c2a77-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

### <a name="data-annotations"></a><span data-ttu-id="c2a77-114">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="c2a77-114">Data Annotations</span></span>

<span data-ttu-id="c2a77-115">Można również zastosować atrybutów (nazywane adnotacji danych) do klasy i właściwości.</span><span class="sxs-lookup"><span data-stu-id="c2a77-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="c2a77-116">Adnotacje danych spowoduje zastąpienie Konwencji, ale zostaną zastąpione przez konfigurację interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="c2a77-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
