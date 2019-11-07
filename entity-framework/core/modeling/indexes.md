---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655695"
---
# <a name="indexes"></a>Zwiększa

Indeksy są typowymi koncepcjami w wielu magazynach danych. Chociaż ich implementacja w magazynie danych może się różnić, są one używane do wykonywania wyszukiwania na podstawie kolumny (lub zestawu kolumn) bardziej wydajne.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją indeks jest tworzony we wszystkich właściwościach (lub zestawach właściwości), które są używane jako klucz obcy.

## <a name="data-annotations"></a>Adnotacje danych

Nie można tworzyć indeksów przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby określić indeks dla pojedynczej właściwości. Domyślnie indeksy nie są unikatowe.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

Można również określić, że indeks powinien być unikatowy, co oznacza, że żadne dwie jednostki nie mogą mieć tych samych wartości dla danej właściwości.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

Można również określić indeks w więcej niż jednej kolumnie.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Istnieje tylko jeden indeks dla każdego odrębnego zestawu właściwości. Jeśli używasz interfejsu API Fluent do skonfigurowania indeksu dla zestawu właściwości, który ma już zdefiniowany indeks, w ramach Konwencji lub poprzedniej konfiguracji, zmienisz definicję tego indeksu. Jest to przydatne, jeśli chcesz skonfigurować indeks, który został utworzony przez Konwencję.
