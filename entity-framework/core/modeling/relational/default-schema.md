---
title: Domyślny schemat — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655967"
---
# <a name="default-schema"></a>Schemat domyślny

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Domyślny schemat to schemat bazy danych, w którym obiekty zostaną utworzone w przypadku, gdy schemat nie jest jawnie skonfigurowany dla tego obiektu.

## <a name="conventions"></a>Konwencje

Według Konwencji dostawca bazy danych wybierze najbardziej odpowiedni domyślny schemat. Na przykład Microsoft SQL Server będzie używać schematu `dbo`, a program SQLite nie użyje schematu (ponieważ schematy nie są obsługiwane w programie SQLite).

## <a name="data-annotations"></a>Adnotacje danych

Nie można ustawić domyślnego schematu przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Możesz użyć interfejsu API Fluent, aby określić domyślny schemat.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
