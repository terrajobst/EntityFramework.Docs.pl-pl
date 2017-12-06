---
title: "Dane — podstawowe EF dotyczące zapisywania"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a><span data-ttu-id="68d10-102">Zapisywanie powiązanych danych</span><span class="sxs-lookup"><span data-stu-id="68d10-102">Saving Related Data</span></span>

<span data-ttu-id="68d10-103">Oprócz izolowanego jednostek można wprowadzić korzystanie z relacji zdefiniowanych w modelu.</span><span class="sxs-lookup"><span data-stu-id="68d10-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="68d10-104">Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="68d10-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="68d10-105">Dodawanie nowych jednostek wykres</span><span class="sxs-lookup"><span data-stu-id="68d10-105">Adding a graph of new entities</span></span>

<span data-ttu-id="68d10-106">Jeśli tworzysz kilka nowych jednostek pokrewnych, dodawanie jeden z nich do kontekstu spowoduje, że inne osoby do dodania zbyt.</span><span class="sxs-lookup"><span data-stu-id="68d10-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="68d10-107">W poniższym przykładzie blogu i trzy powiązane wpisy są wszystkie wstawione do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68d10-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="68d10-108">Znajdowania i dodany, ponieważ są one dostępne za pośrednictwem wpisów `Blog.Posts` właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="68d10-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="68d10-109">Dodawanie jednostki pokrewne</span><span class="sxs-lookup"><span data-stu-id="68d10-109">Adding a related entity</span></span>

<span data-ttu-id="68d10-110">Jeśli nowy obiekt odwołanie z właściwości nawigacji jednostki, która jest już śledzony przez kontekst, jednostki będą wykrywane i wstawione do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68d10-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="68d10-111">W poniższym przykładzie `post` dodaje się jednostki, ponieważ jest ona dodawana do `Posts` właściwość `blog` jednostki, która została pobrana z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68d10-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="68d10-112">Zmiana relacji</span><span class="sxs-lookup"><span data-stu-id="68d10-112">Changing relationships</span></span>

<span data-ttu-id="68d10-113">Jeśli zmienisz właściwości nawigacji jednostki odpowiednie zmiany będą kolumna klucza obcego w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="68d10-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="68d10-114">W poniższym przykładzie `post` jednostki jest aktualizowana w celu należą do nowego `blog` jednostki ponieważ jego `Blog` właściwość nawigacji jest ustawiona, aby wskazywał `blog`.</span><span class="sxs-lookup"><span data-stu-id="68d10-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="68d10-115">Należy pamiętać, że `blog` również zostaną wstawione do bazy danych, ponieważ jest on nową jednostkę, która odwołuje się właściwość nawigacji jednostki, która jest już śledzony przez kontekst (`post`).</span><span class="sxs-lookup"><span data-stu-id="68d10-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="68d10-116">Usuwanie relacji</span><span class="sxs-lookup"><span data-stu-id="68d10-116">Removing relationships</span></span>

<span data-ttu-id="68d10-117">Można usunąć relacji ustawiając nawigacji odwołania `null`, lub usuwanie obiektu pokrewnego z nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="68d10-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="68d10-118">Usuwanie relacji może mieć skutki uboczne w jednostce zależnej, zgodnie z każdym usuwanie zachowanie skonfigurowane w relacji.</span><span class="sxs-lookup"><span data-stu-id="68d10-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="68d10-119">Domyślnie dla wymaganych relacji zachowanie usuwania kaskadowego jest skonfigurowane, a obiekt podrzędny/zależnych zostaną usunięte z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68d10-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="68d10-120">W przypadku relacji opcjonalne, usuwanie kaskadowe nie skonfigurowano domyślnie, ale zostanie ustawiona właściwość klucza obcego do wartości null.</span><span class="sxs-lookup"><span data-stu-id="68d10-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="68d10-121">Zobacz [wymaganych i opcjonalnych relacje](../modeling/relationships.md#required-and-optional-relationships) Aby dowiedzieć się więcej o konfiguracji requiredness relacji.</span><span class="sxs-lookup"><span data-stu-id="68d10-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="68d10-122">Zobacz [Cascade Delete](cascade-delete.md) więcej informacji na temat jak cascade delete zachowania działa jak może być jawnie skonfigurowane i jak są wybrane przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="68d10-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="68d10-123">W poniższym przykładzie usuwanie kaskadowe jest skonfigurowana dla relacji między `Blog` i `Post`, więc `post` jednostki są usuwane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="68d10-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
