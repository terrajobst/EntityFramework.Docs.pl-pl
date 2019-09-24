---
title: Właściwości cienia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197705"
---
# <a name="shadow-properties"></a><span data-ttu-id="6a289-102">Właściwości w tle</span><span class="sxs-lookup"><span data-stu-id="6a289-102">Shadow Properties</span></span>

<span data-ttu-id="6a289-103">Właściwości cienia to właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="6a289-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="6a289-104">Wartość i stan tych właściwości są utrzymywane wyłącznie w monitorze zmian.</span><span class="sxs-lookup"><span data-stu-id="6a289-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="6a289-105">Właściwości cienia są przydatne, gdy w bazie danych znajdują się dane, które nie powinny być uwidocznione w mapowanych typach obiektów.</span><span class="sxs-lookup"><span data-stu-id="6a289-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="6a289-106">Są one najczęściej używane w przypadku właściwości klucza obcego, gdzie relacja między dwiema jednostkami jest reprezentowana przez wartość klucza obcego w bazie danych, ale relacja jest zarządzana w typach jednostek przy użyciu właściwości nawigacji między typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="6a289-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="6a289-107">Wartości właściwości cienia można uzyskać i zmienić za pomocą `ChangeTracker` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6a289-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="6a289-108">Właściwości cienia można przywoływać w zapytaniach `EF.Property` LINQ za pomocą metody statycznej.</span><span class="sxs-lookup"><span data-stu-id="6a289-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="6a289-109">Konwencje</span><span class="sxs-lookup"><span data-stu-id="6a289-109">Conventions</span></span>

<span data-ttu-id="6a289-110">Właściwości cienia można utworzyć według Konwencji, gdy zostanie wykryta relacja, ale w klasie jednostki zależnej nie znaleziono żadnej właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="6a289-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="6a289-111">W takim przypadku zostanie wprowadzona właściwość klucza obcego cienia.</span><span class="sxs-lookup"><span data-stu-id="6a289-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="6a289-112">Właściwość klucza obcego cienia zostanie nazwana `<navigation property name><principal key property name>` (Nawigacja w jednostce zależnej, która wskazuje podmiotowi głównemu, jest używana do nazewnictwa).</span><span class="sxs-lookup"><span data-stu-id="6a289-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="6a289-113">Jeśli nazwa właściwości klucza podmiotu zabezpieczeń zawiera nazwę właściwości nawigacji, nazwa zostanie `<principal key property name>`wydana.</span><span class="sxs-lookup"><span data-stu-id="6a289-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="6a289-114">Jeśli w jednostce zależnej nie ma właściwości nawigacji, w jej miejscu zostanie użyta nazwa typu podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6a289-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="6a289-115">Na przykład następująca lista kodu spowoduje, że właściwość `BlogId` Shadow zostanie wprowadzona `Post` do jednostki.</span><span class="sxs-lookup"><span data-stu-id="6a289-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="6a289-116">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="6a289-116">Data Annotations</span></span>

<span data-ttu-id="6a289-117">Właściwości cienia nie mogą być tworzone za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="6a289-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6a289-118">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="6a289-118">Fluent API</span></span>

<span data-ttu-id="6a289-119">Aby skonfigurować właściwości cienia, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="6a289-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="6a289-120">Po wywołaniu przeciążania `Property` ciągu można utworzyć łańcuch dowolnego wywołania konfiguracji dla innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="6a289-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="6a289-121">Jeśli nazwa przekazywana do `Property` metody jest zgodna z nazwą istniejącej właściwości (właściwości cienia lub jednej zdefiniowanej w klasie jednostki), wówczas kod skonfiguruje istniejącą właściwość zamiast wprowadzać nową właściwość Shadow.</span><span class="sxs-lookup"><span data-stu-id="6a289-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
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
