---
title: Typy danych — podstawowe EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
ms.locfileid: "26054545"
---
# <a name="data-types"></a>Typy danych

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Typ danych odwołuje się do bazy danych określonego typu kolumny, z którą właściwość jest zamapowana.

## <a name="conventions"></a>Konwencje

Według Konwencji dostawcy bazy danych wybiera typ danych na podstawie typu CLR właściwości. Uwzględnia ona, również inne metadane, takie jak skonfigurowanego [maksymalną długość](../max-length.md), czy właściwość jest częścią klucza podstawowego, itd.

Na przykład program SQL Server używa `datetime2(7)` dla `DateTime` właściwości, oraz `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` dla `string` właściwości, które są używane jako klucz).

## <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do określenia typu dokładne dane dla kolumny.

Na przykład następujący kod konfiguruje `Url` jako ciąg z systemem innym niż unicode o maksymalnej długości `200` i `Rating` dziesiętnego z dokładnością do `5` i skalować z `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby określić ten sam typ danych dla kolumny można również Użyj interfejsu API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
