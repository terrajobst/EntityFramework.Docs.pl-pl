---
title: Indeksy (relacyjna baza danych) — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824578"
---
# <a name="indexes-relational-database"></a>Indeksy (relacyjna baza danych)

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Indeks w relacyjnej bazie danych jest mapowany na takie same koncepcje jak indeks w rdzeńu Entity Framework.

## <a name="conventions"></a>Konwencje

Według Konwencji, indeksy mają nazwę `IX_<type name>_<property name>`. W przypadku indeksów złożonych `<property name>` być rozdzielaną podkreśleniem listę nazw właściwości.

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować indeksów przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można skonfigurować nazwę indeksu.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Można również określić filtr.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

W przypadku używania dostawcy SQL Server Dr dodaje filtr `'IS NOT NULL'` dla wszystkich kolumn dopuszczających wartości null, które są częścią unikatowego indeksu. Aby zastąpić tę Konwencję, możesz podać wartość `null`.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Uwzględnij kolumny w indeksach SQL Server

Istnieje możliwość skonfigurowania [indeksów z uwzględnieniem kolumn](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) , aby znacznie poprawić wydajność zapytań, gdy wszystkie kolumny w zapytaniu są uwzględniane w indeksie jako kolumna klucza lub niebędąca kolumną klucza.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
