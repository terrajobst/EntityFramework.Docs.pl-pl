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
# <a name="saving-related-data"></a>Zapisywanie powiązanych danych

Oprócz izolowanych jednostek można również używać relacji zdefiniowanych w modelu.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) użyty w tym artykule można zobaczyć w witrynie GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Dodawanie grafu nowych jednostek

Jeśli tworzysz kilka nowych jednostek pokrewnych, dodanie jednego z nich do kontekstu spowoduje również dodanie innych elementów.

W poniższym przykładzie blog i trzy powiązane wpisy są wstawiane do bazy danych programu. Wpisy są dostępne i dodawane, ponieważ są dostępne za pośrednictwem `Blog.Posts` właściwości nawigacji.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Właściwość EntityEntry. State służy do ustawiania stanu tylko jednej jednostki. Na przykład `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Dodawanie jednostki powiązanej

Jeśli odwołujesz się do nowej jednostki z właściwości nawigacji jednostki, która jest już śledzona przez kontekst, jednostka zostanie odnaleziona i wstawiona do bazy danych.

W poniższym przykładzie `post` jednostka jest wstawiona, ponieważ jest dodawana `Posts` do właściwości jednostki, `blog` która została pobrana z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Zmiana relacji

Jeśli zmienisz właściwość nawigacji jednostki, odpowiednie zmiany zostaną wprowadzone do kolumny klucza obcego w bazie danych.

W poniższym przykładzie `post` jednostka jest aktualizowana, aby należała do nowej `blog` jednostki, ponieważ jej `Blog` właściwość nawigacji jest ustawiona na `blog`wartość. Należy pamiętać `blog` , że program zostanie również wstawiony do bazy danych, ponieważ jest to nowa jednostka, do której odwołuje się właściwość nawigacji jednostki, która jest już śledzona przez`post`kontekst ().

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Usuwanie relacji

Istnieje możliwość usunięcia relacji przez ustawienie nawigacji referencyjnej na `null`lub usunięcie powiązanej jednostki z nawigowania po kolekcji.

Usunięcie relacji może mieć wpływ na jednostkę zależną, zgodnie z zachowaniem usuwania kaskadowego skonfigurowanym w relacji.

Domyślnie w przypadku wymaganych relacji jest konfigurowane zachowanie kaskadowego usuwania, a jednostka podrzędna/zależna zostanie usunięta z bazy danych. W przypadku relacji opcjonalnych usuwanie kaskadowe nie jest domyślnie skonfigurowane, ale właściwość klucza obcego zostanie ustawiona na wartość null.

Zobacz [wymagane i opcjonalne relacje](../modeling/relationships.md#required-and-optional-relationships) , aby dowiedzieć się, jak można skonfigurować wymaganie relacji.

Zobacz [Kaskada Delete](cascade-delete.md) , aby uzyskać więcej informacji na temat działania zachowań kaskadowego usuwania, jak można je jawnie skonfigurować i jak są wybierane przez Konwencję.

W poniższym przykładzie skonfigurowano kaskadowe usuwanie dla relacji między `Blog` i `Post`, więc `post` jednostka zostanie usunięta z bazy danych.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
