---
title: "Indeksy — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a>Indeksy

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Mapuje indeksu relacyjnej bazy danych do tego samego pojęcia jako indeks w podstawowe programu Entity Framework.

## <a name="conventions"></a>Konwencje

Konwencja, indeksy są nazywane `IX_<type name>_<property name>`. Złożone indeksów `<property name>` staje się podkreślenia oddzielone listę nazw właściwości.

## <a name="data-annotations"></a>Adnotacji danych

Indeksy nie można skonfigurować za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować nazwę indeksu, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Można również określić filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Przy użyciu dostawcy programu SQL Server EF dodaje "IS NOT NULL" Filtruj wszystkie kolumny wartości null, które są częścią unikatowego indeksu. Aby zastąpić tę Konwencję, możesz podać `null` wartość.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
