---
title: Dziedziczenie — EF Core
description: Jak skonfigurować dziedziczenie typu jednostki przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502174"
---
# <a name="inheritance"></a><span data-ttu-id="bd532-103">Dziedziczenie</span><span class="sxs-lookup"><span data-stu-id="bd532-103">Inheritance</span></span>

<span data-ttu-id="bd532-104">EF można zmapować hierarchię typów .NET do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bd532-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="bd532-105">Dzięki temu można pisać jednostki platformy .NET w kodzie w zwykły sposób, przy użyciu podstawowych i pochodnych typów, a program EF bezproblemowo tworzy odpowiedni schemat bazy danych, wysyła zapytania itp. Rzeczywiste szczegóły dotyczące sposobu mapowania hierarchii typów są zależne od dostawcy. Ta strona zawiera opis obsługi dziedziczenia w kontekście relacyjnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bd532-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="bd532-106">W tej chwili EF Core obsługuje tylko wzorzec Table-per-Hierarchy (TPH).</span><span class="sxs-lookup"><span data-stu-id="bd532-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="bd532-107">TPH używa pojedynczej tabeli do przechowywania danych dla wszystkich typów w hierarchii, a kolumna rozróżniacza służy do identyfikowania, który typ reprezentuje każdy wiersz.</span><span class="sxs-lookup"><span data-stu-id="bd532-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="bd532-108">EF Core nie są jeszcze obsługiwane przez program na podstawie typu tabeli (TPT) i typu tabeli (TPC), które są obsługiwane przez EF6.</span><span class="sxs-lookup"><span data-stu-id="bd532-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="bd532-109">TPT to główna funkcja zaplanowana dla EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="bd532-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="bd532-110">Mapowanie hierarchii typów jednostek</span><span class="sxs-lookup"><span data-stu-id="bd532-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="bd532-111">Zgodnie z Konwencją EF skonfiguruje tylko dziedziczenie, jeśli dwa lub więcej dziedziczonych typów jest jawnie uwzględnionych w modelu.</span><span class="sxs-lookup"><span data-stu-id="bd532-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="bd532-112">EF nie będzie automatycznie skanować dla typów podstawowych lub pochodnych, które nie są uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="bd532-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="bd532-113">Można uwzględnić typy w modelu, uwidaczniając Nieogólnymi dla każdego typu w hierarchii dziedziczenia:</span><span class="sxs-lookup"><span data-stu-id="bd532-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="bd532-114">Ten model jest mapowany na następujący schemat bazy danych (należy zwrócić uwagę na niejawnie utworzoną kolumnę *rozróżniacza* , która określa, który typ *blogu* jest przechowywany w każdym wierszu):</span><span class="sxs-lookup"><span data-stu-id="bd532-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="bd532-116">W przypadku korzystania z mapowania TPH kolumny bazy danych są automatycznie wprowadzane do wartości null.</span><span class="sxs-lookup"><span data-stu-id="bd532-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="bd532-117">Na przykład kolumna *RssUrl* ma wartość null, ponieważ zwykłe wystąpienia *blogu* nie mają tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="bd532-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="bd532-118">Jeśli nie chcesz uwidaczniać Nieogólnymi dla co najmniej jednej jednostki w hierarchii, możesz również użyć interfejsu API Fluent, aby upewnić się, że są one uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="bd532-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="bd532-119">Jeśli nie korzystasz z Konwencji, możesz określić typ podstawowy jawnie przy użyciu `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="bd532-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="bd532-120">Możesz również użyć `.HasBaseType((Type)null)`, aby usunąć typ jednostki z hierarchii.</span><span class="sxs-lookup"><span data-stu-id="bd532-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="bd532-121">Konfiguracja rozróżniacza</span><span class="sxs-lookup"><span data-stu-id="bd532-121">Discriminator configuration</span></span>

<span data-ttu-id="bd532-122">Można skonfigurować nazwę i typ kolumny rozróżniacza oraz wartości, które są używane do identyfikacji poszczególnych typów w hierarchii:</span><span class="sxs-lookup"><span data-stu-id="bd532-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="bd532-123">W powyższych przykładach EF dodał rozróżniacz niejawnie jako [Właściwość Shadow](xref:core/modeling/shadow-properties) w jednostce podstawowej hierarchii.</span><span class="sxs-lookup"><span data-stu-id="bd532-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="bd532-124">Tę właściwość można skonfigurować w taki sam sposób:</span><span class="sxs-lookup"><span data-stu-id="bd532-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="bd532-125">Na koniec rozróżniacz może być również mapowany na zwykłą Właściwość platformy .NET w jednostce:</span><span class="sxs-lookup"><span data-stu-id="bd532-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="bd532-126">Udostępnione kolumny</span><span class="sxs-lookup"><span data-stu-id="bd532-126">Shared columns</span></span>

<span data-ttu-id="bd532-127">Domyślnie, gdy dwa typy jednostek równorzędnych w hierarchii mają właściwość o tej samej nazwie, zostaną zamapowane na dwie oddzielne kolumny.</span><span class="sxs-lookup"><span data-stu-id="bd532-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="bd532-128">Jeśli jednak ich typ jest identyczny, można go zamapować na tę samą kolumnę bazy danych:</span><span class="sxs-lookup"><span data-stu-id="bd532-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
