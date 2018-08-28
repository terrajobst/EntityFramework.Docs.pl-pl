---
title: Dziedziczenie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995899"
---
# <a name="inheritance"></a><span data-ttu-id="e4040-102">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="e4040-102">Inheritance</span></span>

<span data-ttu-id="e4040-103">Dziedziczenie w modelu platformy EF jest używane do kontrolowania, jak dziedziczenia klas jednostek jest reprezentowana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e4040-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e4040-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="e4040-104">Conventions</span></span>

<span data-ttu-id="e4040-105">Według Konwencji jest dostawca bazy danych, aby określić, jak dziedziczenie będzie reprezentowany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e4040-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="e4040-106">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) dla jak jest to obsługiwane za pomocą dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4040-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="e4040-107">EF skonfiguruje tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy są jawnie uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="e4040-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="e4040-108">EF nie będzie skanować dla typów podstawowych i pochodnych, które w przeciwnym razie nie zostały uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="e4040-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="e4040-109">Mogą zawierać typy w modelu, zapewniając *DbSet<TEntity>*  dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="e4040-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="e4040-110">Jeśli nie chcesz udostępnić *DbSet<TEntity>*  dla co najmniej jednej jednostki w hierarchii można użyć Fluent API, aby upewnić się, że są one uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="e4040-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="e4040-111">A jeśli użytkownik nie należy polegać na konwencjach można określić typ podstawowy, w sposób jawny przy użyciu `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="e4040-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="e4040-112">Możesz użyć `.HasBaseType((Type)null)` można usunąć typu jednostki z hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e4040-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e4040-113">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="e4040-113">Data Annotations</span></span>

<span data-ttu-id="e4040-114">Nie można użyć adnotacji danych, aby skonfigurować dziedziczenie.</span><span class="sxs-lookup"><span data-stu-id="e4040-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e4040-115">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="e4040-115">Fluent API</span></span>

<span data-ttu-id="e4040-116">Interfejs Fluent API dziedziczenia zależy od dostawcy bazy danych, którego używasz.</span><span class="sxs-lookup"><span data-stu-id="e4040-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="e4040-117">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji można wykonać dla dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e4040-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
