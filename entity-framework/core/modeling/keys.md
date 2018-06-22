---
title: Klucze (podstawowe) — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054110"
---
# <a name="keys-primary"></a><span data-ttu-id="c6072-102">Klucze (podstawowy)</span><span class="sxs-lookup"><span data-stu-id="c6072-102">Keys (primary)</span></span>

<span data-ttu-id="c6072-103">Klucz służy jako podstawowy identyfikator unikatowy dla każdego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="c6072-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="c6072-104">Korzystając z relacyjnej bazy danych to mapy do koncepcji *klucz podstawowy*.</span><span class="sxs-lookup"><span data-stu-id="c6072-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="c6072-105">Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="c6072-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="c6072-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c6072-106">Conventions</span></span>

<span data-ttu-id="c6072-107">Konwencja, właściwość o nazwie `Id` lub `<type name>Id` zostaną skonfigurowane jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="c6072-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="c6072-108">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="c6072-108">Data Annotations</span></span>

<span data-ttu-id="c6072-109">Aby skonfigurować jedną właściwość klucza jednostki za, można użyć adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="c6072-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="c6072-110">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="c6072-110">Fluent API</span></span>

<span data-ttu-id="c6072-111">Aby skonfigurować jedną właściwość klucza jednostki za, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="c6072-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="c6072-112">Aby skonfigurować wiele właściwości klucza obiektu (znany jako klucz złożony) umożliwia także interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="c6072-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="c6072-113">Klucze złożone można skonfigurować tylko za pomocą interfejsu API Fluent — konwencje nigdy nie będzie Instalatora klucz złożony i nie umożliwia adnotacji danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c6072-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
