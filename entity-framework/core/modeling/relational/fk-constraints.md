---
title: Ograniczenia klucza obcego — EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824590"
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
