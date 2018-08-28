---
title: Dziedziczenie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995899"
---
# <a name="inheritance"></a>Dziedziczenie

Dziedziczenie w modelu platformy EF jest używane do kontrolowania, jak dziedziczenia klas jednostek jest reprezentowana w bazie danych.

## <a name="conventions"></a>Konwencje

Według Konwencji jest dostawca bazy danych, aby określić, jak dziedziczenie będzie reprezentowany w bazie danych. Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) dla jak jest to obsługiwane za pomocą dostawcy relacyjnej bazy danych.

EF skonfiguruje tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy są jawnie uwzględnione w modelu. EF nie będzie skanować dla typów podstawowych i pochodnych, które w przeciwnym razie nie zostały uwzględnione w modelu. Mogą zawierać typy w modelu, zapewniając *DbSet<TEntity>*  dla każdego typu w hierarchii dziedziczenia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Jeśli nie chcesz udostępnić *DbSet<TEntity>*  dla co najmniej jednej jednostki w hierarchii można użyć Fluent API, aby upewnić się, że są one uwzględnione w modelu.
A jeśli użytkownik nie należy polegać na konwencjach można określić typ podstawowy, w sposób jawny przy użyciu `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Możesz użyć `.HasBaseType((Type)null)` można usunąć typu jednostki z hierarchii.

## <a name="data-annotations"></a>Adnotacje danych

Nie można użyć adnotacji danych, aby skonfigurować dziedziczenie.

## <a name="fluent-api"></a>Interfejs Fluent API

Interfejs Fluent API dziedziczenia zależy od dostawcy bazy danych, którego używasz. Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji można wykonać dla dostawcy relacyjnej bazy danych.
