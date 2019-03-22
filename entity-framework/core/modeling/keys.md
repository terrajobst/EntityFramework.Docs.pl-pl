---
title: Klucze (podstawowe) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 6272e323b83ccab2ed060a2ebbde1d1e8e353d66
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319169"
---
# <a name="keys-primary"></a><span data-ttu-id="7a3e3-102">Klucze (podstawowe)</span><span class="sxs-lookup"><span data-stu-id="7a3e3-102">Keys (primary)</span></span>

<span data-ttu-id="7a3e3-103">Klucz służy jako podstawowy identyfikator unikatowy dla każdego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="7a3e3-104">Korzystając z relacyjnej bazy danych to mapuje do koncepcji *klucz podstawowy*.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="7a3e3-105">Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) Aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="7a3e3-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="7a3e3-106">Jedną z następujących metod może służyć do instalacja/Tworzenie klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="7a3e3-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="7a3e3-107">Conventions</span></span>

<span data-ttu-id="7a3e3-108">Zgodnie z Konwencją, właściwość o nazwie `Id` lub `<type name>Id` zostaną skonfigurowane jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="7a3e3-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="7a3e3-109">Data Annotations</span></span>

<span data-ttu-id="7a3e3-110">Korzystanie z adnotacji danych, aby skonfigurować jedną właściwość jako klucz jednostki.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="7a3e3-111">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="7a3e3-111">Fluent API</span></span>

<span data-ttu-id="7a3e3-112">Interfejs Fluent API umożliwiają skonfigurowanie jednej właściwości klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="7a3e3-113">Można również skonfigurować wiele właściwości, aby być kluczem jednostki (znanych jako klucz złożony) za pomocą Fluent interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="7a3e3-114">Klucze złożone można skonfigurować tylko przy użyciu interfejsu API Fluent — konwencje nigdy nie skonfiguruje klucz złożony i nie umożliwia adnotacje danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7a3e3-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
