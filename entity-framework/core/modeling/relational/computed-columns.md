---
title: Kolumny obliczane — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655920"
---
# <a name="computed-columns"></a>Kolumny obliczane

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Kolumna obliczana to kolumna, której wartość jest obliczana w bazie danych. Kolumna obliczana może użyć innych kolumn w tabeli do obliczenia jej wartości.

## <a name="conventions"></a>Konwencje

Według Konwencji kolumny obliczane nie są tworzone w modelu.

## <a name="data-annotations"></a>Adnotacje danych

Kolumn obliczanych nie można skonfigurować za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby określić, że właściwość powinna być mapowana na kolumnę obliczaną.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
