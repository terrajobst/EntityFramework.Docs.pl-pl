---
title: Sekwencje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656114"
---
# <a name="sequences"></a>Sekwencje

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Sekwencja generuje sekwencyjne wartości liczbowe w bazie danych. Sekwencje nie są skojarzone z określoną tabelą.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją sekwencje nie są wprowadzane w modelu.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować sekwencji przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można utworzyć sekwencję w modelu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Możesz również skonfigurować dodatkowy aspekt sekwencji, taki jak jego schemat, wartość początkowa i przyrost.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Po wprowadzeniu sekwencji można użyć jej do generowania wartości właściwości w modelu. Na przykład można użyć [wartości domyślnych](default-values.md) , aby wstawić następną wartość z sekwencji.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
