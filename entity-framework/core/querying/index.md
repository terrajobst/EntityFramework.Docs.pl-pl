---
title: Wykonywanie zapytania o dane — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949278"
---
# <a name="querying-data"></a>Wykonanie zapytania o dane

Entity Framework Core używa Language Integrated Query (LINQ) do zapytania o dane z bazy danych. LINQ umożliwia przy użyciu języka C# (lub wybranym języku .NET) do pisania zapytań silnie typizowaną oparte na klasach pochodnych kontekstu i jednostki. Reprezentacja zapytania LINQ są przekazywane do dostawcy bazy danych, aby być przetłumaczony na język zapytań specyficznych dla bazy danych (na przykład SQL dla relacyjnej bazy danych). Aby uzyskać bardziej szczegółowe informacje dotyczące sposobu przetwarzania zapytania, zobacz [jak działa zapytanie](overview.md).
