---
title: Dziedziczenie — EF Core
description: Jak skonfigurować dziedziczenie typu jednostki przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824679"
---
# <a name="inheritance"></a><span data-ttu-id="e3b89-103">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="e3b89-103">Inheritance</span></span>

<span data-ttu-id="e3b89-104">Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e3b89-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e3b89-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="e3b89-105">Conventions</span></span>

<span data-ttu-id="e3b89-106">Domyślnie do dostawcy bazy danych można określić, jak dziedziczenie będzie reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="e3b89-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="e3b89-107">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) , aby obsłużyć to działanie za pomocą dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e3b89-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="e3b89-108">EF skonfiguruje tylko dziedziczenie, jeśli dwa lub więcej dziedziczonych typów zostanie jawnie uwzględnionych w modelu.</span><span class="sxs-lookup"><span data-stu-id="e3b89-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="e3b89-109">EF nie skanuje w poszukiwaniu typów podstawowych ani pochodnych, które nie zostały uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="e3b89-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="e3b89-110">Możesz dołączyć typy w modelu, ujawniając *nieogólnymi\<* dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="e3b89-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="e3b89-111">Jeśli nie chcesz uwidocznić *nieogólnymi\<* dla co najmniej jednej jednostki w hierarchii, możesz użyć interfejsu API Fluent, aby upewnić się, że są one uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="e3b89-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="e3b89-112">A jeśli nie korzystasz z Konwencji, możesz określić typ podstawowy jawnie przy użyciu `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="e3b89-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="e3b89-113">Możesz użyć `.HasBaseType((Type)null)`, aby usunąć typ jednostki z hierarchii.</span><span class="sxs-lookup"><span data-stu-id="e3b89-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e3b89-114">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="e3b89-114">Data Annotations</span></span>

<span data-ttu-id="e3b89-115">Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="e3b89-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e3b89-116">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="e3b89-116">Fluent API</span></span>

<span data-ttu-id="e3b89-117">Interfejs API Fluent do dziedziczenia zależy od używanego dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e3b89-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="e3b89-118">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji, którą można wykonać dla dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e3b89-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
