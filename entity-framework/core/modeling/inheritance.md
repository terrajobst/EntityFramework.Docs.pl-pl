---
title: Dziedziczenie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319130"
---
# <a name="inheritance"></a>Dziedziczenie

Dziedziczenie w modelu platformy EF jest używane do kontrolowania, jak dziedziczenia klas jednostek jest reprezentowana w bazie danych.

## <a name="conventions"></a>Konwencje

Według Konwencji jest dostawca bazy danych, aby określić, jak dziedziczenie będzie reprezentowany w bazie danych. Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) dla jak jest to obsługiwane za pomocą dostawcy relacyjnej bazy danych.

EF skonfiguruje tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy są jawnie uwzględnione w modelu. EF nie będzie skanować dla typów podstawowych i pochodnych, które w przeciwnym razie nie zostały uwzględnione w modelu. Mogą zawierać typy w modelu, zapewniając *DbSet<TEntity>*  dla każdego typu w hierarchii dziedziczenia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Jeśli nie chcesz udostępnić *DbSet<TEntity>*  dla co najmniej jednej jednostki w hierarchii można użyć Fluent API, aby upewnić się, że są one uwzględnione w modelu.
A jeśli nie możesz polegać na konwencjach, można określić typ podstawowy, w sposób jawny przy użyciu `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Możesz użyć `.HasBaseType((Type)null)` można usunąć typu jednostki z hierarchii.

## <a name="data-annotations"></a>Adnotacje danych

Nie można użyć adnotacji danych, aby skonfigurować dziedziczenie.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API dziedziczenia zależy od dostawcy bazy danych, którego używasz. Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji można wykonać dla dostawcy relacyjnej bazy danych.
