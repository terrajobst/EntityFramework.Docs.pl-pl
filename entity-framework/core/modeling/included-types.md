---
title: Uwzględnienie & z wyjątkiem typów — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655736"
---
# <a name="including--excluding-types"></a>Uwzględnianie i wykluczanie typów

Uwzględnienie typu w modelu oznacza, że Dr ma metadane dotyczące tego typu i spróbuje odczytywać i zapisywać wystąpienia z/do bazy danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, typy, które są uwidocznione we właściwościach `DbSet` w Twoim kontekście, są uwzględniane w modelu. Ponadto są uwzględnione również typy, które są wymienione w metodzie `OnModelCreating`. Na koniec wszystkie typy, które są znalezione przez rekursywnie Eksplorowanie właściwości nawigacji odnalezionych typów, są również uwzględnione w modelu.

**Na przykład w poniższym kodzie wykryto wszystkie trzy typy:**

* `Blog`, ponieważ jest on uwidoczniony we właściwości `DbSet` w kontekście

* `Post`, ponieważ został on odnaleziony za pośrednictwem właściwości nawigacji `Blog.Posts`

* `AuditEntry`, ponieważ jest on wymieniony w `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby wykluczyć typ z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent do wykluczenia typu z modelu.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
