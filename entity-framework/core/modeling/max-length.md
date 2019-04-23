---
title: Maksymalna długość numeru PIN — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929852"
---
# <a name="maximum-length"></a>Maksymalna długość

Konfigurowanie maksymalnej długości stanowi wskazówkę z magazynem danych o typie danych odpowiednich dla danej właściwości. Maksymalna długość ma zastosowanie tylko do tablicy typów danych, takich jak `string` i `byte[]`.

> [!NOTE]  
> Platformy Entity Framework wykonaj wszelkie weryfikacji o maksymalnej długości przed przekazaniem do dostawcy. Jest magazynu dostawcy lub danych, sprawdź, czy jest to odpowiednie. Na przykład gdy przeznaczonych dla programu SQL Server, która przekracza maksymalną długość spowodują wyjątek jako typ danych kolumny źródłowej nie zezwoli nadmiarowych danych mają być przechowywane.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją pozostało do dostawcy bazy danych, aby wybrać odpowiedni typ danych właściwości. Dla właściwości o długości dostawca bazy danych zazwyczaj wybierz typ danych, który umożliwia najdłuższy długość danych. Na przykład Microsoft SQL Server będzie używać `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` Jeśli kolumna jest używana jako klucz).

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować maksymalną długość dla właściwości. W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` typ danych jest używany.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie maksymalnej długości dla właściwości. W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` typ danych jest używany.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
