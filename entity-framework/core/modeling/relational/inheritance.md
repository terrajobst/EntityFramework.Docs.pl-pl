---
title: Dziedziczenie (relacyjna baza danych) — EF Core
description: Jak skonfigurować dziedziczenie typu jednostki w relacyjnej bazie danych przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824744"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="a91f6-103">Dziedziczenie (relacyjna baza danych)</span><span class="sxs-lookup"><span data-stu-id="a91f6-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="a91f6-104">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="a91f6-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a91f6-105">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="a91f6-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a91f6-106">Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a91f6-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="a91f6-107">Obecnie tylko wzorzec tabeli dla hierarchii (TPH) jest implementowany w EF Core.</span><span class="sxs-lookup"><span data-stu-id="a91f6-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="a91f6-108">Inne typowe wzorce, takie jak Table-per-Type (TPT) i typu tabeli (TPC), nie są jeszcze dostępne.</span><span class="sxs-lookup"><span data-stu-id="a91f6-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="a91f6-109">Konwencje</span><span class="sxs-lookup"><span data-stu-id="a91f6-109">Conventions</span></span>

<span data-ttu-id="a91f6-110">Domyślnie dziedziczenie zostanie zmapowane przy użyciu wzorca tabela-na-hierarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="a91f6-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="a91f6-111">TPH używa pojedynczej tabeli do przechowywania danych dla wszystkich typów w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="a91f6-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="a91f6-112">Kolumna rozróżniacza służy do identyfikowania, który typ reprezentuje każdy wiersz.</span><span class="sxs-lookup"><span data-stu-id="a91f6-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="a91f6-113">EF Core zostanie skonfigurowana tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy zostaną jawnie dołączone do modelu (zobacz [dziedziczenie](../inheritance.md) , aby uzyskać więcej informacji).</span><span class="sxs-lookup"><span data-stu-id="a91f6-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="a91f6-114">Poniżej znajduje się przykład przedstawiający prosty scenariusz dziedziczenia oraz dane przechowywane w tabeli relacyjnej bazy danych przy użyciu wzorca TPH.</span><span class="sxs-lookup"><span data-stu-id="a91f6-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="a91f6-115">Kolumna *rozróżniacza* określa, który typ *blogu* jest przechowywany w każdym wierszu.</span><span class="sxs-lookup"><span data-stu-id="a91f6-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="a91f6-117">W przypadku korzystania z mapowania TPH kolumny bazy danych są automatycznie wprowadzane do wartości null.</span><span class="sxs-lookup"><span data-stu-id="a91f6-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a91f6-118">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="a91f6-118">Data Annotations</span></span>

<span data-ttu-id="a91f6-119">Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.</span><span class="sxs-lookup"><span data-stu-id="a91f6-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a91f6-120">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="a91f6-120">Fluent API</span></span>

<span data-ttu-id="a91f6-121">Korzystając z interfejsu API Fluent, można skonfigurować nazwę i typ kolumny dyskryminatora oraz wartości, które są używane do identyfikowania poszczególnych typów w hierarchii.</span><span class="sxs-lookup"><span data-stu-id="a91f6-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="a91f6-122">Konfigurowanie właściwości rozróżniacza</span><span class="sxs-lookup"><span data-stu-id="a91f6-122">Configuring the discriminator property</span></span>

<span data-ttu-id="a91f6-123">W powyższych przykładach rozróżniacz jest tworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) w jednostce podstawowej hierarchii.</span><span class="sxs-lookup"><span data-stu-id="a91f6-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="a91f6-124">Ponieważ jest to właściwość w modelu, można ją skonfigurować tak samo jak inne właściwości.</span><span class="sxs-lookup"><span data-stu-id="a91f6-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="a91f6-125">Na przykład, aby ustawić maksymalną długość w przypadku użycia domyślnego rozróżniacza Konwencji przez Konwencję:</span><span class="sxs-lookup"><span data-stu-id="a91f6-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="a91f6-126">Rozróżniacz można także zamapować na Właściwość platformy .NET w jednostce i skonfigurować ją.</span><span class="sxs-lookup"><span data-stu-id="a91f6-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="a91f6-127">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a91f6-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="a91f6-128">Udostępnione kolumny</span><span class="sxs-lookup"><span data-stu-id="a91f6-128">Shared columns</span></span>

<span data-ttu-id="a91f6-129">Gdy dwa typy jednostek równorzędnych mają właściwość o tej samej nazwie, zostaną one domyślnie zmapowane do dwóch oddzielnych kolumn.</span><span class="sxs-lookup"><span data-stu-id="a91f6-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="a91f6-130">Ale jeśli są zgodne, można je zamapować na tę samą kolumnę:</span><span class="sxs-lookup"><span data-stu-id="a91f6-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]