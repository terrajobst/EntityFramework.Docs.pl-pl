---
title: Tworzenie i konfigurowanie modelu — EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416416"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="8b493-102">Tworzenie i konfigurowanie modelu</span><span class="sxs-lookup"><span data-stu-id="8b493-102">Creating and configuring a model</span></span>

<span data-ttu-id="8b493-103">Entity Framework używa zestawu konwencji do tworzenia modelu na podstawie kształtu klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="8b493-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="8b493-104">Można określić dodatkową konfigurację, aby uzupełnić i/lub zastąpić to, co zostało wykryte przez konwencję.</span><span class="sxs-lookup"><span data-stu-id="8b493-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="8b493-105">W tym artykule opisano konfigurację, która może być stosowana do modelu docelowego dowolnego magazynu danych i który można zastosować podczas kierowania dowolnej relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8b493-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="8b493-106">Dostawcy mogą również włączyć konfigurację, która jest specyficzna dla określonego magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="8b493-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="8b493-107">Aby uzyskać dokumentację dotyczącą konfiguracji specyficznej dla dostawcy, zobacz sekcję [Dostawcy](../providers/index.md) baz danych.</span><span class="sxs-lookup"><span data-stu-id="8b493-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="8b493-108">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="8b493-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="8b493-109">Konfigurowanie modelu za pomocą płynnego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="8b493-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="8b493-110">Można zastąpić `OnModelCreating` metodę w kontekście pochodnym i `ModelBuilder API` użyć do skonfigurowania modelu.</span><span class="sxs-lookup"><span data-stu-id="8b493-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="8b493-111">Jest to najbardziej zaawansowana metoda konfiguracji i umożliwia konfigurację, która ma być określona bez modyfikowania klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="8b493-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="8b493-112">Płynna konfiguracja interfejsu API ma najwyższy priorytet i zastąpi konwencje i adnotacje danych.</span><span class="sxs-lookup"><span data-stu-id="8b493-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="8b493-113">Konfigurowanie modelu za pomocą adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="8b493-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="8b493-114">Można również zastosować atrybuty (znane jako adnotacje danych) do klas i właściwości.</span><span class="sxs-lookup"><span data-stu-id="8b493-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="8b493-115">Adnotacje danych zastąpią konwencje, ale zostaną zastąpione przez konfigurację interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="8b493-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
