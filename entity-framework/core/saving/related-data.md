---
title: Zapisywanie powiązanych danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 7349c57c0dccd3c911178641d3b34a478a4f6194
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994747"
---
# <a name="saving-related-data"></a>Zapisywanie powiązanych danych

Oprócz izolowanych jednostek można wprowadzić korzystanie z relacji zdefiniowanych w modelu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Dodawanie wykresu nowych jednostek

Jeśli utworzysz kilka nowych jednostek powiązanych, dodając jeden z nich do kontekstu spowoduje, że inne osoby mają zostać dodane za.

W poniższym przykładzie blogu i trzy powiązane wpisy są wszystkie wstawione do bazy danych. Znalezione wpisy i dodawane, ponieważ są one dostępne za pośrednictwem `Blog.Posts` właściwości nawigacji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Właściwość EntityEntry.State służy do ustawiania stanu pojedynczej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Dodawanie powiązanej jednostki

Jeśli odwołujesz Nowa jednostka z właściwości nawigacji jednostki, która jest już śledzony przez kontekst jednostki będą wykrywane i wstawione do bazy danych.

W poniższym przykładzie `post` dodaje się jednostki, ponieważ jest ona dodawana do `Posts` właściwość `blog` jednostki, która została pobrana z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Zmiana relacji

Jeśli zmienisz właściwości nawigacji jednostki, odpowiednie zmiany będą kolumna klucza obcego w bazie danych.

W poniższym przykładzie `post` jednostki zostanie zaktualizowany i będzie należeć do nowego `blog` jednostki ponieważ jej `Blog` właściwość nawigacji jest ustawiona, aby wskazywał `blog`. Należy pamiętać, że `blog` również zostaną wstawione do bazy danych, ponieważ jest nowa jednostka, która odwołuje się do właściwości nawigacji jednostki, która jest już śledzona przez kontekst (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Usuwanie relacji

Możesz usunąć relację, ustawiając nawigacji odwołanie `null`, lub usuwanie obiektu pokrewnego nawigacji kolekcji.

Usunięcie relacji może mieć skutki uboczne jednostki zależne, zgodnie z każdym skonfigurowane w relacji zachowanie przy usuwaniu.

Domyślnie dla wymaganych relacji skonfigurowano zachowanie dotyczące usuwania cascade, a jednostki podrzędne/zależnych od ustawień lokalnych, które zostaną usunięte z bazy danych. W przypadku relacji opcjonalne usuwanie kaskadowe nie skonfigurowano domyślnie, ale zostanie ustawiona właściwość klucza obcego na wartość null.

Zobacz [relacje wymaganych i opcjonalnych](../modeling/relationships.md#required-and-optional-relationships) Aby dowiedzieć się, jak można skonfigurować requiredness relacji.

Zobacz [usuwanie kaskadowe](cascade-delete.md) szczegółowe informacje na temat sposobu usuwanie kaskadowe zachowania działa jak może być jawnie skonfigurowane i jak są wybrane przez Konwencję.

W poniższym przykładzie skonfigurowano usuwanie kaskadowe relacji między `Blog` i `Post`, więc `post` została usunięta z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
