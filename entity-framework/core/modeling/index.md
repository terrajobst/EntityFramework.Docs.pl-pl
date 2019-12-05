---
title: Tworzenie i Konfigurowanie modelu — EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 58be4a45473c6292790da341e360b3340de27be7
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824687"
---
# <a name="creating-and-configuring-a-model"></a>Tworzenie i Konfigurowanie modelu

Entity Framework używa zestawu Konwencji do kompilowania modelu na podstawie kształtu klas jednostek. Możesz określić dodatkową konfigurację, aby uzupełniać i/lub zastępować elementy wykryte przez Konwencję.

W tym artykule opisano konfigurację, którą można zastosować do modelu przeznaczonego dla każdego magazynu danych i które można zastosować w przypadku określania relacyjnej bazy danych. Dostawcy mogą również włączyć konfigurację specyficzną dla określonego magazynu danych. Aby uzyskać dokumentację dotyczącą konfiguracji specyficznej dla dostawcy, zobacz sekcję [dostawcy bazy danych](../providers/index.md) .

> [!TIP]  
>  [Przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Konfigurowanie modelu za pomocą interfejsu API Fluent

Można zastąpić metodę `OnModelCreating` w kontekście pochodnym i skonfigurować model przy użyciu `ModelBuilder API`. Jest to najbardziej wydajna metoda konfiguracji i umożliwia określenie konfiguracji bez modyfikowania klas jednostek. Konfiguracja interfejsu API Fluent ma najwyższy priorytet i zastępuje konwencje i adnotacje danych.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Używanie adnotacji danych do konfigurowania modelu

Można również zastosować atrybuty (znane jako adnotacje danych) do klas i właściwości. Adnotacje danych przesłonią konwencje, ale zostaną przesłonięte przez konfigurację interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
