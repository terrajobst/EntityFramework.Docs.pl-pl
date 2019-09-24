---
title: Mapowanie kolumn — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197215"
---
# <a name="column-mapping"></a>Mapowanie kolumn

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Mapowanie kolumn identyfikuje, które dane kolumny mają być wysyłane i zapisywane w bazie danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją Każda właściwość zostanie skonfigurowana do mapowania do kolumny o tej samej nazwie co właściwość.

## <a name="data-annotations"></a>Adnotacje danych

Możesz użyć adnotacji danych, aby skonfigurować kolumnę, do której zostanie zamapowana właściwość.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować kolumnę, do której jest zamapowana właściwość.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
