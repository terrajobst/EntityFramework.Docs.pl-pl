---
title: Dziedziczenie - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance"></a>Dziedziczenie

Dziedziczenie w modelu EF służy do kontrolowania sposobu dziedziczenia klas jednostek jest reprezentowana w bazie danych.

## <a name="conventions"></a>Konwencje

Według Konwencji jest dostawca bazy danych, aby określić sposób dziedziczenia zostanie odwzorowane w bazie danych. Zobacz [dziedziczenia (relacyjnej bazy danych)](relational/inheritance.md) dla obsługi to u dostawcy relacyjnej bazy danych.

EF tylko umożliwią skonfigurowanie dziedziczenia, jeśli dwa lub więcej dziedziczonych typów jawnie znajdują się w modelu. EF nie będą skanować pod kątem podstawowej lub pochodnych typów, które w przeciwnym razie nie zostały uwzględnione w modelu. Typów można uwzględnić w modelu, w przypadku wystawianego *DbSet<TEntity>*  dla każdego typu w hierarchii dziedziczenia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Jeśli nie chcesz ujawnić *DbSet<TEntity>*  dla jednego lub więcej obiektów w hierarchii można użyć interfejsu API Fluent, aby upewnić się, że znajdują się w modelu.
A jeśli użytkownik nie należy polegać na konwencje można określić typu podstawowego jawnie za pomocą `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Można użyć `.HasBaseType((Type)null)` można usunąć typu jednostki z hierarchii.

## <a name="data-annotations"></a>Adnotacji danych

Za pomocą adnotacji danych nie można skonfigurować dziedziczenia.

## <a name="fluent-api"></a>Interfejsu API Fluent

Interfejsu API Fluent dziedziczenia zależy od dostawcy bazy danych, którego używasz. Zobacz [dziedziczenia (relacyjnej bazy danych)](relational/inheritance.md) konfiguracji można wykonać dla dostawcy relacyjnej bazy danych.
