---
title: Zapisywanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417546"
---
# <a name="saving-related-data"></a><span data-ttu-id="9e165-102">Zapisywanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="9e165-102">Saving Related Data</span></span>

<span data-ttu-id="9e165-103">Oprócz pojedynczych jednostek można również korzystać z relacji zdefiniowanych w modelu.</span><span class="sxs-lookup"><span data-stu-id="9e165-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="9e165-104">Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) przykład artykułu na GitHub.</span><span class="sxs-lookup"><span data-stu-id="9e165-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="9e165-105">Dodawanie wykresu nowych encji</span><span class="sxs-lookup"><span data-stu-id="9e165-105">Adding a graph of new entities</span></span>

<span data-ttu-id="9e165-106">Jeśli utworzysz kilka nowych jednostek pokrewnych, dodanie jednej z nich do kontekstu spowoduje, że inne zostaną dodane.</span><span class="sxs-lookup"><span data-stu-id="9e165-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="9e165-107">W poniższym przykładzie blog i trzy powiązane wpisy są wstawiane do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9e165-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="9e165-108">Posty są znalezione i dodane, ponieważ `Blog.Posts` są one dostępne za pośrednictwem właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9e165-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="9e165-109">Użyj EntityEntry.State właściwości, aby ustawić stan tylko jednej jednostki.</span><span class="sxs-lookup"><span data-stu-id="9e165-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="9e165-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="9e165-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="9e165-111">Dodawanie encji pokrewne</span><span class="sxs-lookup"><span data-stu-id="9e165-111">Adding a related entity</span></span>

<span data-ttu-id="9e165-112">Jeśli odwołujesz się do nowej jednostki z właściwości nawigacji jednostki, która jest już śledzona przez kontekst, jednostka zostanie odnaleziona i wstawiona do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9e165-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="9e165-113">W poniższym przykładzie `post` jednostka jest wstawiana, ponieważ jest dodawana do `Posts` właściwości `blog` jednostki, która została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9e165-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="9e165-114">Zmiana relacji</span><span class="sxs-lookup"><span data-stu-id="9e165-114">Changing relationships</span></span>

<span data-ttu-id="9e165-115">Jeśli zmienisz właściwość nawigacji jednostki, odpowiednie zmiany zostaną wprowadzone do kolumny klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="9e165-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="9e165-116">W poniższym `post` przykładzie jednostka jest aktualizowana, aby należeć do nowej `blog` jednostki, ponieważ jej `Blog` właściwość nawigacji jest ustawiona na `blog`.</span><span class="sxs-lookup"><span data-stu-id="9e165-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="9e165-117">Należy `blog` zauważyć, że zostanie również wstawiony do bazy danych, ponieważ jest to nowa jednostka, do`post`której odwołuje się właściwość nawigacji jednostki, która jest już śledzona przez kontekst ( ).</span><span class="sxs-lookup"><span data-stu-id="9e165-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="9e165-118">Usuwanie relacji</span><span class="sxs-lookup"><span data-stu-id="9e165-118">Removing relationships</span></span>

<span data-ttu-id="9e165-119">Relację można usunąć, ustawiając `null`nawigację referencyjną na element powiązany lub usuwając encję pokrewną z nawigacji z kolekcji.</span><span class="sxs-lookup"><span data-stu-id="9e165-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="9e165-120">Usunięcie relacji może mieć skutki uboczne na encję zależną, zgodnie z zachowaniem usuwania kaskadowego skonfigurowanym w relacji.</span><span class="sxs-lookup"><span data-stu-id="9e165-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="9e165-121">Domyślnie dla wymaganych relacji skonfigurowane jest zachowanie usuwania kaskadowego, a jednostka podrzędna/zależna zostanie usunięta z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9e165-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="9e165-122">W przypadku relacji opcjonalnych usuwanie kaskadowe nie jest domyślnie skonfigurowane, ale właściwość klucza obcego zostanie ustawiona na wartość null.</span><span class="sxs-lookup"><span data-stu-id="9e165-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="9e165-123">Zobacz [Relacje wymagane i opcjonalne,](../modeling/relationships.md#required-and-optional-relationships) aby dowiedzieć się, jak można skonfigurować wymaganie relacji.</span><span class="sxs-lookup"><span data-stu-id="9e165-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="9e165-124">Zobacz [Kaskada Usuń, aby](cascade-delete.md) uzyskać więcej informacji na temat działania zachowań usuwania kaskady, sposobu jawnego konfigurowania i sposobu wybierania ich zgodnie z konwencją.</span><span class="sxs-lookup"><span data-stu-id="9e165-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="9e165-125">W poniższym przykładzie kaskadowe usuwanie jest skonfigurowany `Post`na `post` relacji między `Blog` i , więc jednostka jest usuwany z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9e165-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
