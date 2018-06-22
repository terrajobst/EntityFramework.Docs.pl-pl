---
title: Wykonywanie zapytania na danych - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054215"
---
# <a name="querying-data"></a>Wykonywanie zapytania na danych

Entity Framework Core używa języka integracji zapytania (LINQ) wykonać zapytania o dane z bazy danych. LINQ umożliwia przy użyciu języka C# (lub z języka .NET) do zapisania jednoznacznie zapytań opartych na klas pochodnych kontekstu i jednostki. Reprezentacja zapytania LINQ są przekazywane do dostawcy bazy danych, aby być przetłumaczony na język kwerendy specyficzne dla bazy danych (np. SQL relacyjnej bazy danych). Aby uzyskać bardziej szczegółowe informacje dotyczące sposobu przetwarzania zapytania, zobacz [jak działa zapytanie](overview.md).
