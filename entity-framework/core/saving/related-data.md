---
title: Zapisywanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 45c7b8e4bfa4ce7967ad76ef4a7d4818b0d3aebf
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197890"
---
# <a name="saving-related-data"></a><span data-ttu-id="2d793-102">Zapisywanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="2d793-102">Saving Related Data</span></span>

<span data-ttu-id="2d793-103">Oprócz izolowanych jednostek można również używać relacji zdefiniowanych w modelu.</span><span class="sxs-lookup"><span data-stu-id="2d793-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="2d793-104">[Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) użyty w tym artykule można zobaczyć w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="2d793-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="2d793-105">Dodawanie grafu nowych jednostek</span><span class="sxs-lookup"><span data-stu-id="2d793-105">Adding a graph of new entities</span></span>

<span data-ttu-id="2d793-106">Jeśli tworzysz kilka nowych jednostek pokrewnych, dodanie jednego z nich do kontekstu spowoduje również dodanie innych elementów.</span><span class="sxs-lookup"><span data-stu-id="2d793-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="2d793-107">W poniższym przykładzie blog i trzy powiązane wpisy są wstawiane do bazy danych programu.</span><span class="sxs-lookup"><span data-stu-id="2d793-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="2d793-108">Wpisy są dostępne i dodawane, ponieważ są dostępne za pośrednictwem `Blog.Posts` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="2d793-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="2d793-109">Właściwość EntityEntry. State służy do ustawiania stanu tylko jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="2d793-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="2d793-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="2d793-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="2d793-111">Dodawanie jednostki powiązanej</span><span class="sxs-lookup"><span data-stu-id="2d793-111">Adding a related entity</span></span>

<span data-ttu-id="2d793-112">Jeśli odwołujesz się do nowej jednostki z właściwości nawigacji jednostki, która jest już śledzona przez kontekst, jednostka zostanie odnaleziona i wstawiona do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d793-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="2d793-113">W poniższym przykładzie `post` jednostka jest wstawiona, ponieważ jest dodawana `Posts` do właściwości jednostki, `blog` która została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d793-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="2d793-114">Zmiana relacji</span><span class="sxs-lookup"><span data-stu-id="2d793-114">Changing relationships</span></span>

<span data-ttu-id="2d793-115">Jeśli zmienisz właściwość nawigacji jednostki, odpowiednie zmiany zostaną wprowadzone do kolumny klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2d793-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="2d793-116">W poniższym przykładzie `post` jednostka jest aktualizowana, aby należała do nowej `blog` jednostki, ponieważ jej `Blog` właściwość nawigacji jest ustawiona na `blog`wartość.</span><span class="sxs-lookup"><span data-stu-id="2d793-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="2d793-117">Należy pamiętać `blog` , że program zostanie również wstawiony do bazy danych, ponieważ jest to nowa jednostka, do której odwołuje się właściwość nawigacji jednostki, która jest już śledzona przez`post`kontekst ().</span><span class="sxs-lookup"><span data-stu-id="2d793-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="2d793-118">Usuwanie relacji</span><span class="sxs-lookup"><span data-stu-id="2d793-118">Removing relationships</span></span>

<span data-ttu-id="2d793-119">Istnieje możliwość usunięcia relacji przez ustawienie nawigacji referencyjnej na `null`lub usunięcie powiązanej jednostki z nawigowania po kolekcji.</span><span class="sxs-lookup"><span data-stu-id="2d793-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="2d793-120">Usunięcie relacji może mieć wpływ na jednostkę zależną, zgodnie z zachowaniem usuwania kaskadowego skonfigurowanym w relacji.</span><span class="sxs-lookup"><span data-stu-id="2d793-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="2d793-121">Domyślnie w przypadku wymaganych relacji jest konfigurowane zachowanie kaskadowego usuwania, a jednostka podrzędna/zależna zostanie usunięta z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d793-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="2d793-122">W przypadku relacji opcjonalnych usuwanie kaskadowe nie jest domyślnie skonfigurowane, ale właściwość klucza obcego zostanie ustawiona na wartość null.</span><span class="sxs-lookup"><span data-stu-id="2d793-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="2d793-123">Zobacz [wymagane i opcjonalne relacje](../modeling/relationships.md#required-and-optional-relationships) , aby dowiedzieć się, jak można skonfigurować wymaganie relacji.</span><span class="sxs-lookup"><span data-stu-id="2d793-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="2d793-124">Zobacz [Kaskada Delete](cascade-delete.md) , aby uzyskać więcej informacji na temat działania zachowań kaskadowego usuwania, jak można je jawnie skonfigurować i jak są wybierane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="2d793-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="2d793-125">W poniższym przykładzie skonfigurowano kaskadowe usuwanie dla relacji między `Blog` i `Post`, więc `post` jednostka zostanie usunięta z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="2d793-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
