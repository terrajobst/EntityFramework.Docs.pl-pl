---
title: Klucze podstawowe — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656048"
---
# <a name="primary-keys"></a>Klucze podstawowe

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Dla klucza każdego typu jednostki wprowadzono ograniczenie PRIMARY KEY.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją klucz podstawowy w bazie danych ma nazwę `PK_<type name>`.

## <a name="data-annotations"></a>Adnotacje danych

Brak specyficznych dla relacyjnych baz danych klucza podstawowego można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby skonfigurować nazwę ograniczenia PRIMARY KEY w bazie danych.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
