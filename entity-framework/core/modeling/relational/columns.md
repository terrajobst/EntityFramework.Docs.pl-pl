---
title: Mapowanie kolumny — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929865"
---
# <a name="column-mapping"></a>Mapowanie kolumn

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Mapowanie kolumny określa dane, które kolumny powinien być odpytywane i zapisywane w bazie danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją każda właściwość zostanie skonfigurowana do mapowania kolumny z taką samą nazwę jak właściwość.

## <a name="data-annotations"></a>Adnotacje danych

Korzystanie z adnotacji danych, aby skonfigurować kolumny, z którą właściwość jest zamapowana.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie kolumny, z którą właściwość jest zamapowana.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
