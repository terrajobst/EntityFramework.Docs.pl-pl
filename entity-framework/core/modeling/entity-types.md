---
title: Typy jednostek — EF Core
description: Jak skonfigurować i zmapować typy jednostek przy użyciu Entity Framework Core
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417234"
---
# <a name="entity-types"></a>Typy jednostek

Uwzględnienie Nieogólnymi typu w Twoim kontekście oznacza, że jest ona uwzględniona w modelu EF Core; zwykle odwołujemy się do takiego typu jako *jednostki*. EF Core może odczytywać i zapisywać wystąpienia jednostek z/do bazy danych, a jeśli używana jest relacyjna baza danych, EF Core może tworzyć tabele dla jednostek za pośrednictwem migracji.

## <a name="including-types-in-the-model"></a>Uwzględnianie typów w modelu

Według Konwencji typy, które są ujawniane we właściwościach Nieogólnymi w kontekście, są uwzględniane w modelu jako jednostki. Typy jednostek, które są określone w metodzie `OnModelCreating` są również uwzględniane, podobnie jak wszystkie typy, które są znalezione przez rekursywnie Eksplorowanie właściwości nawigacji innych wykrytych typów jednostek.

W poniższym przykładzie kodu są uwzględniane wszystkie typy:

* `Blog` jest uwzględniona, ponieważ jest uwidoczniona we właściwości Nieogólnymi kontekstu.
* `Post` jest dołączana, ponieważ została odnaleziona za pośrednictwem `Blog.Posts` właściwości nawigacji.
* `AuditEntry`, ponieważ jest określony w `OnModelCreating`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>Wykluczanie typów z modelu

Jeśli nie chcesz dołączać typu do modelu, możesz go wykluczyć:

### <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a>Nazwa tabeli

Według Konwencji każdy typ jednostki zostanie skonfigurowany do mapowania do tabeli bazy danych o tej samej nazwie, co Właściwość Nieogólnymi, która uwidacznia jednostkę. Jeśli dla danej jednostki nie istnieje Nieogólnymi, używana jest nazwa klasy.

Możesz ręcznie skonfigurować nazwę tabeli:

### <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a>Schemat tabeli

W przypadku korzystania z relacyjnej bazy danych tabele są według Konwencji utworzonych w domyślnym schemacie bazy danych. Na przykład Microsoft SQL Server będzie używać schematu `dbo` (SQLite nie obsługuje schematów).

Można skonfigurować tabele do utworzenia w określonym schemacie w następujący sposób:

### <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

Zamiast określać schemat dla każdej tabeli, można również zdefiniować domyślny schemat na poziomie modelu przy użyciu interfejsu API Fluent:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

Należy zauważyć, że ustawienie domyślny schemat wpłynie również na inne obiekty bazy danych, takie jak sekwencje.
