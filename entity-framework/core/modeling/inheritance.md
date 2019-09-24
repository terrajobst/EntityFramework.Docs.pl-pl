---
title: Dziedziczenie — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197288"
---
# <a name="inheritance"></a>Dziedziczenie

Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją, należy do dostawcy bazy danych, aby określić, jak dziedziczenie będzie reprezentowane w bazie danych. Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) , aby obsłużyć to działanie za pomocą dostawcy relacyjnej bazy danych.

EF skonfiguruje tylko dziedziczenie, jeśli dwa lub więcej dziedziczonych typów zostanie jawnie uwzględnionych w modelu. EF nie skanuje w poszukiwaniu typów podstawowych ani pochodnych, które nie zostały uwzględnione w modelu. Możesz dołączyć typy w modelu, uwidaczniając *nieogólnymi<TEntity>*  dla każdego typu w hierarchii dziedziczenia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Jeśli nie chcesz uwidaczniać *nieogólnymi<TEntity>*  dla co najmniej jednej jednostki w hierarchii, możesz użyć interfejsu API Fluent, aby upewnić się, że są one uwzględnione w modelu.
A jeśli nie korzystasz z Konwencji, możesz określić typ podstawowy jawnie przy użyciu `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Można użyć `.HasBaseType((Type)null)` , aby usunąć typ jednostki z hierarchii.

## <a name="data-annotations"></a>Adnotacje danych

Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.

## <a name="fluent-api"></a>Interfejs API Fluent

Interfejs API Fluent do dziedziczenia zależy od używanego dostawcy bazy danych. Zobacz [dziedziczenie (relacyjna baza danych)](relational/inheritance.md) konfiguracji, którą można wykonać dla dostawcy relacyjnej bazy danych.
