---
title: Klucze (podstawowe) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197270"
---
# <a name="keys-primary"></a><span data-ttu-id="b8b06-102">Klucze (podstawowe)</span><span class="sxs-lookup"><span data-stu-id="b8b06-102">Keys (primary)</span></span>

<span data-ttu-id="b8b06-103">Klucz służy jako podstawowy unikatowy identyfikator dla każdego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="b8b06-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="b8b06-104">W przypadku korzystania z relacyjnej bazy danych mapowania na koncepcję *klucza podstawowego*.</span><span class="sxs-lookup"><span data-stu-id="b8b06-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="b8b06-105">Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="b8b06-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="b8b06-106">Za pomocą jednej z poniższych metod można skonfigurować/utworzyć klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="b8b06-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="b8b06-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="b8b06-107">Conventions</span></span>

<span data-ttu-id="b8b06-108">Zgodnie z Konwencją właściwość o `Id` nazwie `<type name>Id` lub zostanie skonfigurowana jako klucz jednostki.</span><span class="sxs-lookup"><span data-stu-id="b8b06-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="b8b06-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="b8b06-109">Data Annotations</span></span>

<span data-ttu-id="b8b06-110">Możesz użyć adnotacji danych do skonfigurowania pojedynczej właściwości jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="b8b06-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="b8b06-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="b8b06-111">Fluent API</span></span>

<span data-ttu-id="b8b06-112">Interfejs API Fluent służy do konfigurowania pojedynczej właściwości jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="b8b06-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="b8b06-113">Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz jednostki (nazywanego kluczem złożonym).</span><span class="sxs-lookup"><span data-stu-id="b8b06-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="b8b06-114">Klucze złożone można skonfigurować tylko przy użyciu konwencji API Fluent nigdy nie konfiguruje klucza złożonego i nie można użyć adnotacji danych do skonfigurowania.</span><span class="sxs-lookup"><span data-stu-id="b8b06-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
