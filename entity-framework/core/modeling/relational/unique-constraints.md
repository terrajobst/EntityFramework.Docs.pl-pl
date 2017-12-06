---
title: "Klucze alternatywne (ograniczenia Unique) — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a>Klucze alternatywne (ograniczenia Unique)

> [!NOTE]  
> Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie. Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).

Wprowadzono unikatowego ograniczenia dla każdego alternatywnego klucza w modelu.

## <a name="conventions"></a>Konwencje

Według Konwencji, będzie miała nazwę indeksu i ograniczenia, które zostały wprowadzone dla klucza alternatywnego `AK_<type name>_<property name>`. Złożonych kluczy alternatywnych `<property name>` staje się podkreślenia oddzielone listę nazw właściwości.

## <a name="data-annotations"></a>Adnotacji danych

Unikalne ograniczenia nie można skonfigurować za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejsu API Fluent

Aby skonfigurować nazwę indeksu i ograniczenie klucza alternatywnego, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
