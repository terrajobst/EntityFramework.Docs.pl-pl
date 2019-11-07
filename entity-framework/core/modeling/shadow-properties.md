---
title: Właściwości cienia — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655955"
---
# <a name="shadow-properties"></a>Właściwości w tle

Właściwości cienia to właściwości, które nie są zdefiniowane w klasie jednostki .NET, ale są zdefiniowane dla tego typu jednostki w modelu EF Core. Wartość i stan tych właściwości są utrzymywane wyłącznie w monitorze zmian.

Właściwości cienia są przydatne, gdy w bazie danych znajdują się dane, które nie powinny być uwidocznione w mapowanych typach obiektów. Są one najczęściej używane w przypadku właściwości klucza obcego, gdzie relacja między dwiema jednostkami jest reprezentowana przez wartość klucza obcego w bazie danych, ale relacja jest zarządzana w typach jednostek przy użyciu właściwości nawigacji między typami jednostek.

Wartości właściwości cienia można uzyskać i zmienić za pomocą interfejsu API `ChangeTracker`.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Właściwości cienia można przywoływać w zapytaniach LINQ za pomocą metody statycznej `EF.Property`.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konwencje

Właściwości cienia można utworzyć według Konwencji, gdy zostanie wykryta relacja, ale w klasie jednostki zależnej nie znaleziono żadnej właściwości klucza obcego. W takim przypadku zostanie wprowadzona właściwość klucza obcego cienia. Właściwość klucza obcego cienia będzie nazywana `<navigation property name><principal key property name>` (Nawigacja w jednostce zależnej, która wskazuje jednostkę główną, jest używana do nazewnictwa). Jeśli nazwa właściwości klucza podmiotu zabezpieczeń zawiera nazwę właściwości nawigacji, nazwa zostanie po prostu `<principal key property name>`. Jeśli w jednostce zależnej nie ma właściwości nawigacji, w jej miejscu zostanie użyta nazwa typu podmiotu zabezpieczeń.

Na przykład następująca lista kodu spowoduje wprowadzenie `BlogId` właściwości cienia do jednostki `Post`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Adnotacje danych

Właściwości cienia nie mogą być tworzone za pomocą adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Aby skonfigurować właściwości cienia, można użyć interfejsu API Fluent. Po wywołaniu ciągu przeciążenia `Property` można utworzyć łańcuch dowolnego wywołania konfiguracji dla innych właściwości.

Jeśli nazwa podana do metody `Property` jest zgodna z nazwą istniejącej właściwości (właściwości cienia lub jednej zdefiniowanej w klasie jednostki), wówczas kod skonfiguruje istniejącą właściwość zamiast wprowadzać nową właściwość Shadow.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
