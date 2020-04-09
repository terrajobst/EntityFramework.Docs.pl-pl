---
title: Tworzenie i konfigurowanie modelu — EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416416"
---
# <a name="creating-and-configuring-a-model"></a>Tworzenie i konfigurowanie modelu

Entity Framework używa zestawu konwencji do tworzenia modelu na podstawie kształtu klas jednostek. Można określić dodatkową konfigurację, aby uzupełnić i/lub zastąpić to, co zostało wykryte przez konwencję.

W tym artykule opisano konfigurację, która może być stosowana do modelu docelowego dowolnego magazynu danych i który można zastosować podczas kierowania dowolnej relacyjnej bazy danych. Dostawcy mogą również włączyć konfigurację, która jest specyficzna dla określonego magazynu danych. Aby uzyskać dokumentację dotyczącą konfiguracji specyficznej dla dostawcy, zobacz sekcję [Dostawcy](../providers/index.md) baz danych.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) przykład artykułu na GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Konfigurowanie modelu za pomocą płynnego interfejsu API

Można zastąpić `OnModelCreating` metodę w kontekście pochodnym i `ModelBuilder API` użyć do skonfigurowania modelu. Jest to najbardziej zaawansowana metoda konfiguracji i umożliwia konfigurację, która ma być określona bez modyfikowania klas jednostek. Płynna konfiguracja interfejsu API ma najwyższy priorytet i zastąpi konwencje i adnotacje danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Konfigurowanie modelu za pomocą adnotacji danych

Można również zastosować atrybuty (znane jako adnotacje danych) do klas i właściwości. Adnotacje danych zastąpią konwencje, ale zostaną zastąpione przez konfigurację interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
