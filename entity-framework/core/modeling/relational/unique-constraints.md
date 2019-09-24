---
title: Klucze alternatywne (ograniczenia UNIQUE) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197615"
---
# <a name="alternate-keys-unique-constraints"></a>Klucze alternatywne (ograniczenia unikatowe)

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Dla każdego alternatywnego klucza w modelu jest wprowadzane unikatowe ograniczenie.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją indeks i ograniczenie wprowadzone dla klucza alternatywnego będą nazwane `AK_<type name>_<property name>`. Dla złożone klucze `<property name>` alternatywne są rozdzielaną podkreśleniem listą nazw właściwości.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować ograniczeń unique przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować indeks i nazwę ograniczenia klucza alternatywnego.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
