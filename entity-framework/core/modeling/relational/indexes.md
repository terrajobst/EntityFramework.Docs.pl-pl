---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197018"
---
# <a name="indexes"></a>Zwiększa

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Indeks w relacyjnej bazie danych jest mapowany na takie same koncepcje jak indeks w rdzeńu Entity Framework.

## <a name="conventions"></a>Konwencje

Według Konwencji, indeksy są nazywane `IX_<type name>_<property name>`. W przypadku indeksów `<property name>` złożonych zostaje rozdzielona podkreśleniem listę nazw właściwości.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować indeksów przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można skonfigurować nazwę indeksu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Można również określić filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

W przypadku korzystania z dostawcy SQL Server Dr dodaje filtr "IS NOT NULL" dla wszystkich kolumn dopuszczających wartości null, które są częścią unikatowego indeksu. Aby zastąpić tę Konwencję, możesz podać `null` wartość.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Uwzględnij kolumny w indeksach SQL Server

Istnieje możliwość skonfigurowania [indeksów z uwzględnieniem kolumn](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) , aby znacznie poprawić wydajność zapytań, gdy wszystkie kolumny w zapytaniu są uwzględniane w indeksie jako kolumna klucza lub niebędąca kolumną klucza.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
