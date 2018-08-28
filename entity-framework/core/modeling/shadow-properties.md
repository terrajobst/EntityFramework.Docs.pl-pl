---
title: Właściwości w tle — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993805"
---
# <a name="shadow-properties"></a><span data-ttu-id="f7186-102">Właściwości w tle</span><span class="sxs-lookup"><span data-stu-id="f7186-102">Shadow Properties</span></span>

<span data-ttu-id="f7186-103">Właściwości w tle są właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu platformy EF Core.</span><span class="sxs-lookup"><span data-stu-id="f7186-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="f7186-104">Wartość i stan tych właściwości jest obsługiwane wyłącznie w śledzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="f7186-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="f7186-105">Właściwości w tle są przydatne w przypadku danych w bazie danych, które nie powinny zostać ujawnione w typach zamapowanego jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7186-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="f7186-106">W większości przypadków są one używane dla właściwości klucza obcego, których relację między dwiema jednostkami jest reprezentowany przez wartość klucza obcego w bazie danych, lecz relację odbywa się na typy jednostek przy użyciu właściwości nawigacji między typami encji.</span><span class="sxs-lookup"><span data-stu-id="f7186-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="f7186-107">Wartości właściwości w tle, które mogą być uzyskane i zmienić za pomocą `ChangeTracker` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f7186-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="f7186-108">Właściwości w tle mogą być przywoływane w zapytaniach LINQ za pośrednictwem `EF.Property` metody statycznej.</span><span class="sxs-lookup"><span data-stu-id="f7186-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="f7186-109">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f7186-109">Conventions</span></span>

<span data-ttu-id="f7186-110">Właściwości w tle mogą być tworzone przez Konwencję, gdy relacji został odnaleziony, ale nie właściwość klucza obcego znajduje się w klasie jednostki zależne.</span><span class="sxs-lookup"><span data-stu-id="f7186-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="f7186-111">W takim przypadku zostaną wprowadzone właściwości klucza obcego w tle.</span><span class="sxs-lookup"><span data-stu-id="f7186-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="f7186-112">Właściwość klucza obcego w tle będą miały nazwę nadaną `<navigation property name><principal key property name>` (nawigacji na jednostki zależne wskazuje główną jednostki, używany na potrzeby nazywania).</span><span class="sxs-lookup"><span data-stu-id="f7186-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="f7186-113">Jeśli nazwa właściwości klucza podmiotu zabezpieczeń zawiera nazwę właściwości nawigacji, a następnie po prostu nazwą będzie `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f7186-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="f7186-114">Jeśli nie ma właściwości nawigacji jednostki zależne, Nazwa Typ podmiotu zabezpieczeń jest używana w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="f7186-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="f7186-115">Na przykład w poniższym fragmencie kodu spowoduje `BlogId` właściwości w tle są wprowadzane do `Post` jednostki.</span><span class="sxs-lookup"><span data-stu-id="f7186-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="f7186-116">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f7186-116">Data Annotations</span></span>

<span data-ttu-id="f7186-117">Nie można utworzyć właściwości w tle przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="f7186-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f7186-118">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="f7186-118">Fluent API</span></span>

<span data-ttu-id="f7186-119">Interfejs Fluent API umożliwiają skonfigurowanie właściwości w tle.</span><span class="sxs-lookup"><span data-stu-id="f7186-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="f7186-120">Gdy wywołujesz przeciążenie ciągu `Property` można połączyć w łańcuch dowolne wywołania konfiguracji, jak w przypadku innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="f7186-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="f7186-121">Jeśli nazwa dostarczona do `Property` metody jest zgodna z nazwą istniejącej właściwości (właściwość w tle lub jeden zdefiniowany w klasie jednostek), a następnie kod służy do konfigurowania właściwości istniejących, zamiast wprowadzać nowe właściwości w tle.</span><span class="sxs-lookup"><span data-stu-id="f7186-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
