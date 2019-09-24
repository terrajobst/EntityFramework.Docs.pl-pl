---
title: Typy danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197180"
---
# <a name="data-types"></a>Typy danych

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Typ danych odnosi się do typu określonego dla bazy danych kolumny, do której jest zamapowana właściwość.

## <a name="conventions"></a>Konwencje

Według Konwencji dostawca bazy danych wybiera typ danych na podstawie typu .NET właściwości. Uwzględnia również inne metadane, takie jak skonfigurowana [Maksymalna długość](../max-length.md), czy właściwość jest częścią klucza podstawowego itd.

Na przykład SQL Server używa `datetime2(7)` dla `DateTime` właściwości i `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` dla `string` właściwości, które są używane jako klucz).

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby określić dokładny typ danych dla kolumny.

Na przykład poniższy kod konfiguruje `Url` jako ciąg niebędący znakiem Unicode z maksymalną `200` długością i `Rating` `2`jako dziesiętną `5` z dokładnością i skalą.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz również użyć interfejsu API Fluent, aby określić te same typy danych dla kolumn.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
