---
title: Klucze alternatywne (ograniczenia unikatowe) - programu EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994194"
---
# <a name="alternate-keys-unique-constraints"></a>Klucze alternatywne (ograniczenia unikatowe)

> [!NOTE]  
> Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji. Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).

Ograniczenia unique został wprowadzony dla każdego klucza alternatywnego w modelu.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, indeksu i ograniczenia, które zostały wprowadzone dla klucza alternatywnego będą miały nazwę nadaną `AK_<type name>_<property name>`. Klucze alternatywne złożonego `<property name>` staje się listę nazw właściwości oddzielonych znakiem podkreślenia.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować ograniczeń UNIQUE przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API umożliwiają skonfigurowanie nazwę indeksu i ograniczenie klucza alternatywnego.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
