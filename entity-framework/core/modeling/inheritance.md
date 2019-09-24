---
title: Dziedziczenie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197288"
---
# <a name="inheritance"></a><span data-ttu-id="364e5-102">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="364e5-102">Inheritance</span></span>

<span data-ttu-id="364e5-103">Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="364e5-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="364e5-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="364e5-104">Conventions</span></span>

<span data-ttu-id="364e5-105">Zgodnie z Konwencją, należy do dostawcy bazy danych, aby określić, jak dziedziczenie będzie reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="364e5-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="364e5-106">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) , aby obsłużyć to działanie za pomocą dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="364e5-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="364e5-107">EF skonfiguruje tylko dziedziczenie, jeśli dwa lub więcej dziedziczonych typów zostanie jawnie uwzględnionych w modelu.</span><span class="sxs-lookup"><span data-stu-id="364e5-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="364e5-108">EF nie skanuje w poszukiwaniu typów podstawowych ani pochodnych, które nie zostały uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="364e5-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="364e5-109">Możesz dołączyć typy w modelu, uwidaczniając *nieogólnymi<TEntity>*  dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="364e5-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="364e5-110">Jeśli nie chcesz uwidaczniać *nieogólnymi<TEntity>*  dla co najmniej jednej jednostki w hierarchii, możesz użyć interfejsu API Fluent, aby upewnić się, że są one uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="364e5-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="364e5-111">A jeśli nie korzystasz z Konwencji, możesz określić typ podstawowy jawnie przy użyciu `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="364e5-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="364e5-112">Można użyć `.HasBaseType((Type)null)` , aby usunąć typ jednostki z hierarchii.</span><span class="sxs-lookup"><span data-stu-id="364e5-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="364e5-113">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="364e5-113">Data Annotations</span></span>

<span data-ttu-id="364e5-114">Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="364e5-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="364e5-115">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="364e5-115">Fluent API</span></span>

<span data-ttu-id="364e5-116">Interfejs API Fluent do dziedziczenia zależy od używanego dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="364e5-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="364e5-117">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji, którą można wykonać dla dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="364e5-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
