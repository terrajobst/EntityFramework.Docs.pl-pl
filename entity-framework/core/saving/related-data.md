---
title: Dane — podstawowe EF dotyczące zapisywania
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a><span data-ttu-id="46d50-102">Zapisywanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="46d50-102">Saving Related Data</span></span>

<span data-ttu-id="46d50-103">Oprócz izolowanego jednostek można wprowadzić korzystanie z relacji zdefiniowanych w modelu.</span><span class="sxs-lookup"><span data-stu-id="46d50-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="46d50-104">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="46d50-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="46d50-105">Dodawanie nowych jednostek wykres</span><span class="sxs-lookup"><span data-stu-id="46d50-105">Adding a graph of new entities</span></span>

<span data-ttu-id="46d50-106">Jeśli tworzysz kilka nowych jednostek pokrewnych, dodawanie jeden z nich do kontekstu spowoduje, że inne osoby do dodania zbyt.</span><span class="sxs-lookup"><span data-stu-id="46d50-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="46d50-107">W poniższym przykładzie blogu i trzy powiązane wpisy są wszystkie wstawione do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="46d50-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="46d50-108">Znajdowania i dodany, ponieważ są one dostępne za pośrednictwem wpisów `Blog.Posts` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="46d50-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="46d50-109">Właściwość EntityEntry.State służy do ustawiania stanu pojedynczej jednostki.</span><span class="sxs-lookup"><span data-stu-id="46d50-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="46d50-110">Na przykład `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="46d50-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="46d50-111">Dodawanie jednostki pokrewne</span><span class="sxs-lookup"><span data-stu-id="46d50-111">Adding a related entity</span></span>

<span data-ttu-id="46d50-112">Jeśli nowy obiekt odwołanie z właściwości nawigacji jednostki, która jest już śledzony przez kontekst, jednostki będą wykrywane i wstawione do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="46d50-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="46d50-113">W poniższym przykładzie `post` dodaje się jednostki, ponieważ jest ona dodawana do `Posts` właściwość `blog` jednostki, która została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="46d50-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="46d50-114">Zmiana relacji</span><span class="sxs-lookup"><span data-stu-id="46d50-114">Changing relationships</span></span>

<span data-ttu-id="46d50-115">Jeśli zmienisz właściwości nawigacji jednostki odpowiednie zmiany będą kolumna klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="46d50-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="46d50-116">W poniższym przykładzie `post` jednostki jest aktualizowana w celu należą do nowego `blog` jednostki ponieważ jego `Blog` właściwość nawigacji jest ustawiona, aby wskazywał `blog`.</span><span class="sxs-lookup"><span data-stu-id="46d50-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="46d50-117">Należy pamiętać, że `blog` również zostaną wstawione do bazy danych, ponieważ jest on nową jednostkę, która odwołuje się właściwość nawigacji jednostki, która jest już śledzony przez kontekst (`post`).</span><span class="sxs-lookup"><span data-stu-id="46d50-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="46d50-118">Usuwanie relacji</span><span class="sxs-lookup"><span data-stu-id="46d50-118">Removing relationships</span></span>

<span data-ttu-id="46d50-119">Można usunąć relacji ustawiając nawigacji odwołania `null`, lub usuwanie obiektu pokrewnego z nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="46d50-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="46d50-120">Usuwanie relacji może mieć skutki uboczne w jednostce zależnej, zgodnie z każdym usuwanie zachowanie skonfigurowane w relacji.</span><span class="sxs-lookup"><span data-stu-id="46d50-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="46d50-121">Domyślnie dla wymaganych relacji zachowanie usuwania kaskadowego jest skonfigurowane, a obiekt podrzędny/zależnych zostaną usunięte z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="46d50-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="46d50-122">W przypadku relacji opcjonalne, usuwanie kaskadowe nie skonfigurowano domyślnie, ale zostanie ustawiona właściwość klucza obcego do wartości null.</span><span class="sxs-lookup"><span data-stu-id="46d50-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="46d50-123">Zobacz [wymaganych i opcjonalnych relacje](../modeling/relationships.md#required-and-optional-relationships) Aby dowiedzieć się więcej o konfiguracji requiredness relacji.</span><span class="sxs-lookup"><span data-stu-id="46d50-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="46d50-124">Zobacz [Cascade Delete](cascade-delete.md) więcej informacji na temat jak cascade delete zachowania działa jak może być jawnie skonfigurowane i jak są wybrane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="46d50-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="46d50-125">W poniższym przykładzie usuwanie kaskadowe jest skonfigurowana dla relacji między `Blog` i `Post`, więc `post` jednostki są usuwane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="46d50-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
