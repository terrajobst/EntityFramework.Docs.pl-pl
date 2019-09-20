---
title: Dostawca Azure Cosmos DB — ograniczenia — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150810"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Ograniczenia dostawcy Azure Cosmos DB EF Core

Dostawca Cosmos ma pewne ograniczenia. Wiele z tych ograniczeń jest wynikiem ograniczeń w źródłowym aparacie bazy danych Cosmos i nie są specyficzne dla EF. Ale większość po prostu [nie została jeszcze zaimplementowana](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Ograniczenia tymczasowe

- Nawet jeśli istnieje tylko jeden typ jednostki bez dziedziczenia zamapowanego do kontenera, nadal ma właściwość rozróżniacza.
- Typy jednostek z kluczami partycji nie działają prawidłowo w niektórych scenariuszach
- `Include`wywołania nie są obsługiwane
- `Join`wywołania nie są obsługiwane

## <a name="azure-cosmos-db-sdk-limitations"></a>Ograniczenia dotyczące Azure Cosmos DB SDK

- Podano tylko metody asynchroniczne

> [!WARNING]
> Ponieważ nie istnieją żadne wersje synchronizacji metod niskiego poziomu, EF Core opierają się na tym, że odpowiednie funkcje są obecnie implementowane przez `.Wait()` wywołanie `Task`zwróconych wartości. Oznacza to, że użycie metod `SaveChanges`takich jak `ToList` , lub zamiast ich odpowiedników asynchronicznych może prowadzić do zakleszczenia w aplikacji

## <a name="azure-cosmos-db-limitations"></a>Ograniczenia Azure Cosmos DB

Aby zapoznać się z pełnym omówieniem [Azure Cosmos DB obsługiwanych funkcji](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), są to najbardziej znaczące różnice w porównaniu do relacyjnej bazy danych:

- Transakcje inicjowane przez klienta nie są obsługiwane
- Niektóre zapytania między partycjami nie są obsługiwane lub są znacznie wolniejsze w zależności od operatorów