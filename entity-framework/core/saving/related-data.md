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
# <a name="saving-related-data"></a>Zapisywanie powiązanych danych

Oprócz pojedynczych jednostek można również korzystać z relacji zdefiniowanych w modelu.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) przykład artykułu na GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Dodawanie wykresu nowych encji

Jeśli utworzysz kilka nowych jednostek pokrewnych, dodanie jednej z nich do kontekstu spowoduje, że inne zostaną dodane.

W poniższym przykładzie blog i trzy powiązane wpisy są wstawiane do bazy danych. Posty są znalezione i dodane, ponieważ `Blog.Posts` są one dostępne za pośrednictwem właściwości nawigacji.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Użyj EntityEntry.State właściwości, aby ustawić stan tylko jednej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Dodawanie encji pokrewne

Jeśli odwołujesz się do nowej jednostki z właściwości nawigacji jednostki, która jest już śledzona przez kontekst, jednostka zostanie odnaleziona i wstawiona do bazy danych.

W poniższym przykładzie `post` jednostka jest wstawiana, ponieważ jest dodawana do `Posts` właściwości `blog` jednostki, która została pobrana z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Zmiana relacji

Jeśli zmienisz właściwość nawigacji jednostki, odpowiednie zmiany zostaną wprowadzone do kolumny klucza obcego w bazie danych.

W poniższym `post` przykładzie jednostka jest aktualizowana, aby należeć do nowej `blog` jednostki, ponieważ jej `Blog` właściwość nawigacji jest ustawiona na `blog`. Należy `blog` zauważyć, że zostanie również wstawiony do bazy danych, ponieważ jest to nowa jednostka, do`post`której odwołuje się właściwość nawigacji jednostki, która jest już śledzona przez kontekst ( ).

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Usuwanie relacji

Relację można usunąć, ustawiając `null`nawigację referencyjną na element powiązany lub usuwając encję pokrewną z nawigacji z kolekcji.

Usunięcie relacji może mieć skutki uboczne na encję zależną, zgodnie z zachowaniem usuwania kaskadowego skonfigurowanym w relacji.

Domyślnie dla wymaganych relacji skonfigurowane jest zachowanie usuwania kaskadowego, a jednostka podrzędna/zależna zostanie usunięta z bazy danych. W przypadku relacji opcjonalnych usuwanie kaskadowe nie jest domyślnie skonfigurowane, ale właściwość klucza obcego zostanie ustawiona na wartość null.

Zobacz [Relacje wymagane i opcjonalne,](../modeling/relationships.md#required-and-optional-relationships) aby dowiedzieć się, jak można skonfigurować wymaganie relacji.

Zobacz [Kaskada Usuń, aby](cascade-delete.md) uzyskać więcej informacji na temat działania zachowań usuwania kaskady, sposobu jawnego konfigurowania i sposobu wybierania ich zgodnie z konwencją.

W poniższym przykładzie kaskadowe usuwanie jest skonfigurowany `Post`na `post` relacji między `Blog` i , więc jednostka jest usuwany z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
