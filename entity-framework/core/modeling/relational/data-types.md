---
title: Typy danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993524"
---
# <a name="data-types"></a>Typy danych

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Typ danych odnosi się do bazy danych określonego typu kolumny, z którą właściwość jest zamapowana.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją dostawcy bazy danych wybiera typ danych, w zależności od typu CLR właściwości. On uwzględnia również inne metadane, takie jak skonfigurowanych [maksymalną długość](../max-length.md), czy właściwość jest częścią klucza podstawowego, itp.

Na przykład program SQL Server używa `datetime2(7)` dla `DateTime` właściwości, a `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` dla `string` właściwości, które są używane jako klucz).

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby określić dokładny typ danych dla kolumny.

Na przykład poniższy kod służy do konfigurowania `Url` jako ciąg znaków innego niż unicode o maksymalnej długości `200` i `Rating` jako dziesiętna z dokładnością do `5` i skalowanie z `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Interfejs Fluent API

Można również określić ten sam typ danych dla kolumn za pomocą Fluent interfejsu API.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
