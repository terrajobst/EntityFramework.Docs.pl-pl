---
title: Tworzenie i Konfigurowanie modelu — EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 58be4a45473c6292790da341e360b3340de27be7
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824687"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="aa6a5-102">Tworzenie i Konfigurowanie modelu</span><span class="sxs-lookup"><span data-stu-id="aa6a5-102">Creating and configuring a model</span></span>

<span data-ttu-id="aa6a5-103">Entity Framework używa zestawu Konwencji do kompilowania modelu na podstawie kształtu klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="aa6a5-104">Możesz określić dodatkową konfigurację, aby uzupełniać i/lub zastępować elementy wykryte przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="aa6a5-105">W tym artykule opisano konfigurację, którą można zastosować do modelu przeznaczonego dla każdego magazynu danych i które można zastosować w przypadku określania relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="aa6a5-106">Dostawcy mogą również włączyć konfigurację specyficzną dla określonego magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="aa6a5-107">Aby uzyskać dokumentację dotyczącą konfiguracji specyficznej dla dostawcy, zobacz sekcję [dostawcy bazy danych](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="aa6a5-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="aa6a5-108"> [Przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) tego artykułu można wyświetlić w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="aa6a5-109">Konfigurowanie modelu za pomocą interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="aa6a5-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="aa6a5-110">Można zastąpić metodę `OnModelCreating` w kontekście pochodnym i skonfigurować model przy użyciu `ModelBuilder API`.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="aa6a5-111">Jest to najbardziej wydajna metoda konfiguracji i umożliwia określenie konfiguracji bez modyfikowania klas jednostek.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="aa6a5-112">Konfiguracja interfejsu API Fluent ma najwyższy priorytet i zastępuje konwencje i adnotacje danych.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="aa6a5-113">Używanie adnotacji danych do konfigurowania modelu</span><span class="sxs-lookup"><span data-stu-id="aa6a5-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="aa6a5-114">Można również zastosować atrybuty (znane jako adnotacje danych) do klas i właściwości.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="aa6a5-115">Adnotacje danych przesłonią konwencje, ale zostaną przesłonięte przez konfigurację interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="aa6a5-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
