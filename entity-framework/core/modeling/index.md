---
title: Tworzenie modelu — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 67012d0f52cc77ce872fc428fccc20526f3fefad
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489274"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="2ff63-102">Tworzenie i konfigurowanie modelu</span><span class="sxs-lookup"><span data-stu-id="2ff63-102">Creating and configuring a Model</span></span>

<span data-ttu-id="2ff63-103">Entity Framework używa zestawu Konwencji do zbudowania modelu oparte na kształt klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="2ff63-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="2ff63-104">Możesz określić dodatkowej konfiguracji w celu uzupełnienia i/lub zastąpić, co zostało wykryte przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="2ff63-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="2ff63-105">W tym artykule opisano konfiguracji, które mogą być stosowane do modelu, przeznaczone dla dowolnego magazynu danych i tych, które można zastosować w przypadku przeznaczone dla dowolnej relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2ff63-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="2ff63-106">Dostawców może też umożliwiać konfiguracji, które są specyficzne dla określonego magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="2ff63-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="2ff63-107">Dokumentację dotyczącą konfiguracji określonego dostawcy znaleźć [dostawcy baz danych](../providers/index.md) sekcji.</span><span class="sxs-lookup"><span data-stu-id="2ff63-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="2ff63-108">Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2ff63-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="2ff63-109">Użyj interfejsu API fluent, aby skonfigurować model</span><span class="sxs-lookup"><span data-stu-id="2ff63-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="2ff63-110">Można zastąpić `OnModelCreating` metodę w pochodnej kontekstu i użyj `ModelBuilder API` do skonfigurowania modelu.</span><span class="sxs-lookup"><span data-stu-id="2ff63-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="2ff63-111">To jest najbardziej zaawansowane metody konfiguracji i umożliwia konfigurację można określić bez konieczności wprowadzania zmian w Twoich zajęciach jednostki.</span><span class="sxs-lookup"><span data-stu-id="2ff63-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="2ff63-112">Konfiguracja interfejsu API Fluent ma najwyższy priorytet i spowoduje zastąpienie danych i konwencje adnotacji.</span><span class="sxs-lookup"><span data-stu-id="2ff63-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="2ff63-113">Umożliwia konfigurowanie modelu adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="2ff63-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="2ff63-114">Atrybuty (znanych jako adnotacje danych) można zastosować także do klas i właściwości.</span><span class="sxs-lookup"><span data-stu-id="2ff63-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="2ff63-115">Adnotacje danych spowoduje zastąpienie Konwencji, ale zostaną zastąpione przez interfejs Fluent API konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2ff63-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
