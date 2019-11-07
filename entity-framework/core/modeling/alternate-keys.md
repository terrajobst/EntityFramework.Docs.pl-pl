---
title: Klucze alternatywne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655776"
---
# <a name="alternate-keys"></a>Klucze alternatywne

Alternatywny klucz służy jako alternatywny unikatowy identyfikator dla każdego wystąpienia jednostki oprócz klucza podstawowego. Klucze alternatywne mogą służyć jako element docelowy relacji. W przypadku korzystania z relacyjnej bazy danych mapowanie do koncepcji unikatowego indeksu/ograniczenia w kolumnach klucza alternatywnego oraz co najmniej jedno ograniczenie klucza obcego odwołujące się do kolumn.

> [!TIP]  
> Jeśli chcesz, aby wymusić unikatowość kolumny, a następnie chcesz użyć indeksu unikatowego, a nie klucza alternatywnego, zobacz [indeksy](indexes.md). W EF klucze alternatywne zapewniają większą funkcjonalność niż indeksy unikatowe, ponieważ mogą być używane jako obiekty docelowe klucza obcego.

Klucze alternatywne są zwykle wprowadzane w razie potrzeby i nie trzeba ich ręcznie konfigurować. Aby uzyskać więcej informacji, zobacz [konwencje](#conventions) .

## <a name="conventions"></a>Konwencje

Przy użyciu konwencji klucz alternatywny jest wprowadzany w przypadku identyfikowania właściwości, która nie jest kluczem podstawowym jako elementu docelowego relacji.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Adnotacje danych

Nie można skonfigurować kluczy alternatywnych przy użyciu adnotacji danych.

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować pojedynczą właściwość jako klucz alternatywny.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

Możesz również użyć interfejsu API Fluent, aby skonfigurować wiele właściwości jako klucz alternatywny (nazywany kluczem alternatywnym).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
