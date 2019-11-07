---
title: Klucze (podstawowe) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655945"
---
# <a name="keys-primary"></a><span data-ttu-id="01bf9-102">Klucze (podstawowe)</span><span class="sxs-lookup"><span data-stu-id="01bf9-102">Keys (primary)</span></span>

<span data-ttu-id="01bf9-103">Klucz służy jako podstawowy unikatowy identyfikator dla każdego wystąpienia jednostki.</span><span class="sxs-lookup"><span data-stu-id="01bf9-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="01bf9-104">W przypadku korzystania z relacyjnej bazy danych mapowania na koncepcję *klucza podstawowego*.</span><span class="sxs-lookup"><span data-stu-id="01bf9-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="01bf9-105">Można również skonfigurować unikatowy identyfikator, który nie jest kluczem podstawowym (zobacz [klucze alternatywne](alternate-keys.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="01bf9-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="01bf9-106">Za pomocą jednej z poniższych metod można skonfigurować/utworzyć klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="01bf9-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="01bf9-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="01bf9-107">Conventions</span></span>

<span data-ttu-id="01bf9-108">Zgodnie z Konwencją właściwość o nazwie `Id` lub `<type name>Id` zostanie skonfigurowana jako klucz jednostki.</span><span class="sxs-lookup"><span data-stu-id="01bf9-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="01bf9-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="01bf9-109">Data Annotations</span></span>

<span data-ttu-id="01bf9-110">Możesz użyć adnotacji danych do skonfigurowania pojedynczej właściwości jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="01bf9-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="01bf9-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="01bf9-111">Fluent API</span></span>

<span data-ttu-id="01bf9-112">Interfejs API Fluent służy do konfigurowania pojedynczej właściwości jako klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="01bf9-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="01bf9-113">Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz jednostki (nazywanego kluczem złożonym).</span><span class="sxs-lookup"><span data-stu-id="01bf9-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="01bf9-114">Klucze złożone można skonfigurować tylko przy użyciu konwencji API Fluent nigdy nie konfiguruje klucza złożonego i nie można użyć adnotacji danych do skonfigurowania.</span><span class="sxs-lookup"><span data-stu-id="01bf9-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
