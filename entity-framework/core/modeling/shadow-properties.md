---
title: Właściwości cienia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655955"
---
# <a name="shadow-properties"></a><span data-ttu-id="f218a-102">Właściwości w tle</span><span class="sxs-lookup"><span data-stu-id="f218a-102">Shadow Properties</span></span>

<span data-ttu-id="f218a-103">Właściwości cienia to właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu EF Core.</span><span class="sxs-lookup"><span data-stu-id="f218a-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="f218a-104">Wartość i stan tych właściwości są utrzymywane wyłącznie w monitorze zmian.</span><span class="sxs-lookup"><span data-stu-id="f218a-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="f218a-105">Właściwości cienia są przydatne, gdy w bazie danych znajdują się dane, które nie powinny być uwidocznione w mapowanych typach obiektów.</span><span class="sxs-lookup"><span data-stu-id="f218a-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="f218a-106">Są one najczęściej używane w przypadku właściwości klucza obcego, gdzie relacja między dwiema jednostkami jest reprezentowana przez wartość klucza obcego w bazie danych, ale relacja jest zarządzana w typach jednostek przy użyciu właściwości nawigacji między typami jednostek.</span><span class="sxs-lookup"><span data-stu-id="f218a-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="f218a-107">Wartości właściwości cienia można uzyskać i zmienić za pomocą interfejsu API `ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="f218a-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="f218a-108">Właściwości cienia można przywoływać w zapytaniach LINQ za pomocą metody statycznej `EF.Property`.</span><span class="sxs-lookup"><span data-stu-id="f218a-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="f218a-109">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f218a-109">Conventions</span></span>

<span data-ttu-id="f218a-110">Właściwości cienia można utworzyć według Konwencji, gdy zostanie wykryta relacja, ale w klasie jednostki zależnej nie znaleziono żadnej właściwości klucza obcego.</span><span class="sxs-lookup"><span data-stu-id="f218a-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="f218a-111">W takim przypadku zostanie wprowadzona właściwość klucza obcego cienia.</span><span class="sxs-lookup"><span data-stu-id="f218a-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="f218a-112">Właściwość klucza obcego cienia będzie nazywana `<navigation property name><principal key property name>` (Nawigacja w jednostce zależnej, która wskazuje jednostkę główną, jest używana do nazewnictwa).</span><span class="sxs-lookup"><span data-stu-id="f218a-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="f218a-113">Jeśli nazwa właściwości klucza podmiotu zabezpieczeń zawiera nazwę właściwości nawigacji, nazwa zostanie po prostu `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="f218a-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="f218a-114">Jeśli w jednostce zależnej nie ma właściwości nawigacji, w jej miejscu zostanie użyta nazwa typu podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="f218a-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="f218a-115">Na przykład następująca lista kodu spowoduje wprowadzenie `BlogId` właściwości cienia do jednostki `Post`.</span><span class="sxs-lookup"><span data-stu-id="f218a-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a><span data-ttu-id="f218a-116">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f218a-116">Data Annotations</span></span>

<span data-ttu-id="f218a-117">Właściwości cienia nie mogą być tworzone za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="f218a-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f218a-118">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="f218a-118">Fluent API</span></span>

<span data-ttu-id="f218a-119">Aby skonfigurować właściwości cienia, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f218a-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="f218a-120">Po wywołaniu ciągu przeciążenia `Property` można utworzyć łańcuch dowolnego wywołania konfiguracji dla innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="f218a-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="f218a-121">Jeśli nazwa podana do metody `Property` jest zgodna z nazwą istniejącej właściwości (właściwości cienia lub jednej zdefiniowanej w klasie jednostki), wówczas kod skonfiguruje istniejącą właściwość zamiast wprowadzać nową właściwość Shadow.</span><span class="sxs-lookup"><span data-stu-id="f218a-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
