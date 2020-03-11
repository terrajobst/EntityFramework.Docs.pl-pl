---
title: Dostawca Azure Cosmos DB — ograniczenia — EF Core
description: Ograniczenia dostawcy Azure Cosmos DB Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417210"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Ograniczenia dostawcy Azure Cosmos DB EF Core

Dostawca Cosmos ma pewne ograniczenia. Wiele z tych ograniczeń jest wynikiem ograniczeń w źródłowym aparacie bazy danych Cosmos i nie są specyficzne dla EF. Ale większość po prostu [nie została jeszcze zaimplementowana](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Ograniczenia tymczasowe

- Nawet jeśli istnieje tylko jeden typ jednostki bez dziedziczenia zamapowanego do kontenera, nadal ma właściwość rozróżniacza.
- Typy jednostek z kluczami partycji nie działają prawidłowo w niektórych scenariuszach
- wywołania `Include` nie są obsługiwane
- wywołania `Join` nie są obsługiwane

## <a name="azure-cosmos-db-sdk-limitations"></a>Ograniczenia dotyczące Azure Cosmos DB SDK

- Podano tylko metody asynchroniczne

> [!WARNING]
> Ponieważ nie istnieją żadne wersje synchronizacji metod niskiego poziomu EF Core opierają się na tym, że odpowiednie funkcje są obecnie implementowane przez wywoływanie `.Wait()` na zwracanym `Task`. Oznacza to, że użycie metod, takich jak `SaveChanges`, lub `ToList` zamiast ich odpowiedników asynchronicznych może prowadzić do zakleszczenia w aplikacji

## <a name="azure-cosmos-db-limitations"></a>Ograniczenia Azure Cosmos DB

Aby zapoznać się z pełnym omówieniem [Azure Cosmos DB obsługiwanych funkcji](/azure/cosmos-db/modeling-data), są to najbardziej znaczące różnice w porównaniu do relacyjnej bazy danych:

- Transakcje inicjowane przez klienta nie są obsługiwane
- Niektóre zapytania między partycjami nie są obsługiwane lub są znacznie wolniejsze w zależności od operatorów
