---
title: Dziedziczenie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655684"
---
# <a name="inheritance"></a><span data-ttu-id="8ff6b-102">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="8ff6b-102">Inheritance</span></span>

<span data-ttu-id="8ff6b-103">Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="8ff6b-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="8ff6b-104">Conventions</span></span>

<span data-ttu-id="8ff6b-105">Zgodnie z Konwencją, należy do dostawcy bazy danych, aby określić, jak dziedziczenie będzie reprezentowane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="8ff6b-106">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) , aby obsłużyć to działanie za pomocą dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="8ff6b-107">EF skonfiguruje tylko dziedziczenie, jeśli dwa lub więcej dziedziczonych typów zostanie jawnie uwzględnionych w modelu.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="8ff6b-108">EF nie skanuje w poszukiwaniu typów podstawowych ani pochodnych, które nie zostały uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="8ff6b-109">Możesz dołączyć typy w modelu, ujawniając *nieogólnymi\<* dla każdego typu w hierarchii dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-109">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="8ff6b-110">Jeśli nie chcesz uwidocznić *nieogólnymi\<* dla co najmniej jednej jednostki w hierarchii, możesz użyć interfejsu API Fluent, aby upewnić się, że są one uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-110">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="8ff6b-111">A jeśli nie korzystasz z Konwencji, możesz określić typ podstawowy jawnie przy użyciu `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="8ff6b-112">Możesz użyć `.HasBaseType((Type)null)`, aby usunąć typ jednostki z hierarchii.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8ff6b-113">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="8ff6b-113">Data Annotations</span></span>

<span data-ttu-id="8ff6b-114">Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8ff6b-115">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="8ff6b-115">Fluent API</span></span>

<span data-ttu-id="8ff6b-116">Interfejs API Fluent do dziedziczenia zależy od używanego dostawcy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="8ff6b-117">Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji, którą można wykonać dla dostawcy relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8ff6b-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
