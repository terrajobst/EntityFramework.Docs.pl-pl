---
title: "Właściwości — podstawowe EF w tle"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a><span data-ttu-id="b486b-102">Właściwości w tle</span><span class="sxs-lookup"><span data-stu-id="b486b-102">Shadow Properties</span></span>

<span data-ttu-id="b486b-103">Właściwości cienia są właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu podstawowej EF.</span><span class="sxs-lookup"><span data-stu-id="b486b-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="b486b-104">Wartość i stan tych właściwości są obsługiwane wyłącznie w śledzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="b486b-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="b486b-105">Właściwości w tle są przydatne, gdy brak danych w bazie danych, które nie powinny zostać ujawnione w typach jednostek mapowane.</span><span class="sxs-lookup"><span data-stu-id="b486b-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="b486b-106">Są one najczęściej używane dla właściwości klucza obcego, gdzie relację między dwiema jednostkami jest reprezentowany przez wartość klucza obcego w bazie danych, ale relacji jest zarządzana w przypadku typów jednostek przy użyciu właściwości nawigacji między typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="b486b-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="b486b-107">Wartości właściwości w tle mogą być uzyskane i zmieniać za pomocą `ChangeTracker` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="b486b-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="b486b-108">Właściwości w tle może być przywoływany w zapytań LINQ za pośrednictwem `EF.Property` metody statycznej.</span><span class="sxs-lookup"><span data-stu-id="b486b-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="b486b-109">Konwencje</span><span class="sxs-lookup"><span data-stu-id="b486b-109">Conventions</span></span>

<span data-ttu-id="b486b-110">Właściwości w tle można tworzyć przez Konwencję, gdy zostanie wykryta relacji, ale ma właściwości klucza obcego znajduje się w klasie jednostki zależnej.</span><span class="sxs-lookup"><span data-stu-id="b486b-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="b486b-111">W takim przypadku zostaną wprowadzone tle właściwości kluczy obcych.</span><span class="sxs-lookup"><span data-stu-id="b486b-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="b486b-112">Właściwość klucza obcego cień będzie miała nazwę `<navigation property name><principal key property name>` (nawigacji na jednostki zależnej wskazuje główną jednostki, jest używany dla nazwy).</span><span class="sxs-lookup"><span data-stu-id="b486b-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="b486b-113">Jeśli nazwa główna właściwości klucza zawierają nazwę właściwości nawigacji, a następnie nazwę będzie działać `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="b486b-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="b486b-114">Jeśli nie ma właściwości nawigacji na jednostki zależnej, nazwa typu podmiotu zabezpieczeń jest używany w jego miejscu.</span><span class="sxs-lookup"><span data-stu-id="b486b-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="b486b-115">Na przykład w poniższym fragmencie kodu spowoduje `BlogId` wprowadzeniem do właściwości tle `Post` jednostki.</span><span class="sxs-lookup"><span data-stu-id="b486b-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="b486b-116">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="b486b-116">Data Annotations</span></span>

<span data-ttu-id="b486b-117">Nie można utworzyć właściwości tle przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="b486b-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b486b-118">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="b486b-118">Fluent API</span></span>

<span data-ttu-id="b486b-119">Można skonfigurować właściwości w tle, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b486b-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="b486b-120">Po wywołaniu przeciążenia ciąg `Property` można łańcucha żadnego z tych wywołań konfiguracji jak dla innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="b486b-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="b486b-121">Jeśli nazwa dostarczona do `Property` metoda zgodna z nazwą istniejącej właściwości (właściwość tle lub zdefiniowanych w klasie jednostek), a następnie kod służy do konfigurowania tego istniejącej właściwości, zamiast wprowadzać nową właściwość w tle.</span><span class="sxs-lookup"><span data-stu-id="b486b-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
