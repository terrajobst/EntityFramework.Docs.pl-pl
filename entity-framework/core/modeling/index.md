---
title: Tworzenie modelu — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 78a8ffd2393a914edf737104f14e41f8a9074ad5
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929901"
---
# <a name="creating-and-configuring-a-model"></a>Tworzenie i konfigurowanie modelu

Entity Framework używa zestawu Konwencji do zbudowania modelu oparte na kształt klas jednostek. Możesz określić dodatkowej konfiguracji w celu uzupełnienia i/lub zastąpić, co zostało wykryte przez Konwencję.

W tym artykule opisano konfiguracji, które mogą być stosowane do modelu, przeznaczone dla dowolnego magazynu danych i tych, które można zastosować w przypadku przeznaczone dla dowolnej relacyjnej bazy danych. Dostawców może też umożliwiać konfiguracji, które są specyficzne dla określonego magazynu danych. Dokumentację dotyczącą konfiguracji określonego dostawcy znaleźć [dostawcy baz danych](../providers/index.md) sekcji.

> [!TIP]  
> Można wyświetlić w tym artykule [przykładowe](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) w witrynie GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Użyj interfejsu API fluent, aby skonfigurować model

Można zastąpić `OnModelCreating` metodę w pochodnej kontekstu i użyj `ModelBuilder API` do skonfigurowania modelu. To jest najbardziej zaawansowane metody konfiguracji i umożliwia konfigurację można określić bez konieczności wprowadzania zmian w Twoich zajęciach jednostki. Konfiguracja interfejsu API Fluent ma najwyższy priorytet i spowoduje zastąpienie danych i konwencje adnotacji.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a>Umożliwia konfigurowanie modelu adnotacji danych

Atrybuty (znanych jako adnotacje danych) można zastosować także do klas i właściwości. Adnotacje danych spowoduje zastąpienie Konwencji, ale zostaną zastąpione przez interfejs Fluent API konfiguracji.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]
