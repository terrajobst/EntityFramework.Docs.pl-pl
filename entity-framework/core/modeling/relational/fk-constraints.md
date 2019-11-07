---
title: Ograniczenia klucza obcego — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655996"
---
# <a name="foreign-key-constraints"></a>Ograniczenia klucza obcego

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Ograniczenie klucza obcego jest wprowadzane dla każdej relacji w modelu.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją ograniczenia klucza obcego mają nazwę `FK_<dependent type name>_<principal type name>_<foreign key property name>`. W przypadku złożonych kluczy obcych `<foreign key property name>` stać się rozdzielaną podkreśleniem listę nazw właściwości klucza obcego.

## <a name="data-annotations"></a>Adnotacje danych

Nazw ograniczeń klucza obcego nie można skonfigurować przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można skonfigurować nazwę ograniczenia klucza obcego dla relacji.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
