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
# <a name="saving-related-data"></a>Zapisywanie powiązanych danych

Oprócz izolowanego jednostek można wprowadzić korzystanie z relacji zdefiniowanych w modelu.

> [!TIP]  
> Można wyświetlić w tym artykule [próbki](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) w witrynie GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Dodawanie nowych jednostek wykres

Jeśli tworzysz kilka nowych jednostek pokrewnych, dodawanie jeden z nich do kontekstu spowoduje, że inne osoby do dodania zbyt.

W poniższym przykładzie blogu i trzy powiązane wpisy są wszystkie wstawione do bazy danych. Znajdowania i dodany, ponieważ są one dostępne za pośrednictwem wpisów `Blog.Posts` właściwości nawigacji.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Właściwość EntityEntry.State służy do ustawiania stanu pojedynczej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Dodawanie jednostki pokrewne

Jeśli nowy obiekt odwołanie z właściwości nawigacji jednostki, która jest już śledzony przez kontekst, jednostki będą wykrywane i wstawione do bazy danych.

W poniższym przykładzie `post` dodaje się jednostki, ponieważ jest ona dodawana do `Posts` właściwość `blog` jednostki, która została pobrana z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Zmiana relacji

Jeśli zmienisz właściwości nawigacji jednostki odpowiednie zmiany będą kolumna klucza obcego w bazie danych.

W poniższym przykładzie `post` jednostki jest aktualizowana w celu należą do nowego `blog` jednostki ponieważ jego `Blog` właściwość nawigacji jest ustawiona, aby wskazywał `blog`. Należy pamiętać, że `blog` również zostaną wstawione do bazy danych, ponieważ jest on nową jednostkę, która odwołuje się właściwość nawigacji jednostki, która jest już śledzony przez kontekst (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Usuwanie relacji

Można usunąć relacji ustawiając nawigacji odwołania `null`, lub usuwanie obiektu pokrewnego z nawigacji kolekcji.

Usuwanie relacji może mieć skutki uboczne w jednostce zależnej, zgodnie z każdym usuwanie zachowanie skonfigurowane w relacji.

Domyślnie dla wymaganych relacji zachowanie usuwania kaskadowego jest skonfigurowane, a obiekt podrzędny/zależnych zostaną usunięte z bazy danych. W przypadku relacji opcjonalne, usuwanie kaskadowe nie skonfigurowano domyślnie, ale zostanie ustawiona właściwość klucza obcego do wartości null.

Zobacz [wymaganych i opcjonalnych relacje](../modeling/relationships.md#required-and-optional-relationships) Aby dowiedzieć się więcej o konfiguracji requiredness relacji.

Zobacz [Cascade Delete](cascade-delete.md) więcej informacji na temat jak cascade delete zachowania działa jak może być jawnie skonfigurowane i jak są wybrane przez Konwencję.

W poniższym przykładzie usuwanie kaskadowe jest skonfigurowana dla relacji między `Blog` i `Post`, więc `post` jednostki są usuwane z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
