---
title: Dziedziczenie - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054197"
---
# <a name="inheritance"></a><span data-ttu-id="3288c-102">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="3288c-102">Inheritance</span></span>

<span data-ttu-id="3288c-103">Dziedziczenie w modelu EF służy do kontrolowania sposobu dziedziczenia klas jednostek jest reprezentowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3288c-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3288c-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="3288c-104">Conventions</span></span>

<span data-ttu-id="3288c-105">Według Konwencji jest dostawca bazy danych, aby określić sposób dziedziczenia zostanie odwzorowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3288c-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="3288c-106">Zobacz [dziedziczenia (relacyjnej bazy danych)](relational/inheritance.md) dla obsługi to u dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3288c-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="3288c-107">EF tylko umożliwią skonfigurowanie dziedziczenia, jeśli dwa lub więcej dziedziczonych typów jawnie znajdują się w modelu.</span><span class="sxs-lookup"><span data-stu-id="3288c-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="3288c-108">EF nie będą skanować pod kątem podstawowej lub pochodnych typów, które w przeciwnym razie nie zostały uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="3288c-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="3288c-109">Typów można uwzględnić w modelu, w przypadku wystawianego *DbSet<TEntity>*  dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="3288c-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="3288c-110">Jeśli nie chcesz ujawnić *DbSet<TEntity>*  dla jednego lub więcej obiektów w hierarchii można użyć interfejsu API Fluent, aby upewnić się, że znajdują się w modelu.</span><span class="sxs-lookup"><span data-stu-id="3288c-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="3288c-111">A jeśli użytkownik nie należy polegać na konwencje można określić typu podstawowego jawnie za pomocą `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="3288c-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="3288c-112">Można użyć `.HasBaseType((Type)null)` można usunąć typu jednostki z hierarchii.</span><span class="sxs-lookup"><span data-stu-id="3288c-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3288c-113">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="3288c-113">Data Annotations</span></span>

<span data-ttu-id="3288c-114">Za pomocą adnotacji danych nie można skonfigurować dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="3288c-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3288c-115">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="3288c-115">Fluent API</span></span>

<span data-ttu-id="3288c-116">Interfejsu API Fluent dziedziczenia zależy od dostawcy bazy danych, którego używasz.</span><span class="sxs-lookup"><span data-stu-id="3288c-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="3288c-117">Zobacz [dziedziczenia (relacyjnej bazy danych)](relational/inheritance.md) konfiguracji można wykonać dla dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3288c-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
