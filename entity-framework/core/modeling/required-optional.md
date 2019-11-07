---
title: Właściwości wymagane i opcjonalne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655662"
---
# <a name="required-and-optional-properties"></a>Właściwości wymagane i opcjonalne

Właściwość jest uważana za opcjonalną, jeśli jest poprawna, aby zawierała `null`. Jeśli `null` nie jest prawidłową wartością do przypisania do właściwości, zostanie ona uznana za właściwość wymaganą.

Podczas mapowania na schemat relacyjnej bazy danych, wymagane właściwości są tworzone jako kolumny niedopuszczające wartości null, a właściwości opcjonalne są tworzone jako kolumny dopuszczające wartości null.

## <a name="conventions"></a>Konwencje

Zgodnie z Konwencją właściwość, której typ .NET może zawierać wartość null, zostanie skonfigurowana jako opcjonalna, natomiast właściwości, których typ .NET nie może zawierać wartości null, zostaną skonfigurowane zgodnie z wymaganiami. Na przykład wszystkie właściwości z typami wartości .NET (`int`, `decimal`, `bool`itp.) są skonfigurowane jako wymagane, a wszystkie właściwości z typami wartości null platformy .NET (`int?`, `decimal?`, `bool?`itp.) są konfigurowane jako opcjonalne.

C#8 wprowadzono nową funkcję o nazwie [typu referencyjnego nullable](/dotnet/csharp/tutorials/nullable-reference-types), która pozwala na dodawanie adnotacji do typów referencyjnych, wskazując, czy są one prawidłowe, aby zawierały wartość null. Ta funkcja jest domyślnie wyłączona, a jeśli ta opcja jest włączona, modyfikuje zachowanie EF Core w następujący sposób:

* Jeśli typy odwołań do wartości null są wyłączone (wartość domyślna), wszystkie właściwości z typami odwołań platformy .NET są konfigurowane jako opcjonalne według Konwencji (np. `string`).
* Jeśli typy odwołań do wartości null są włączone, właściwości zostaną skonfigurowane na podstawie C# wartości null typu .net: `string?` zostanie skonfigurowany jako opcjonalny, a `string` zostanie skonfigurowany zgodnie z wymaganiami.

W poniższym przykładzie przedstawiono typ jednostki z wymaganymi i opcjonalnymi właściwościami z włączoną funkcją odwołania do wartości null (ustawienie domyślne) i włączony:

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Bez wartości null typów referencyjnych (wartość domyślna)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[Z typami odwołań dopuszczających wartość null](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Zaleca się użycie typów referencyjnych dopuszczających wartość null, ponieważ powoduje to C# przeznaczenie wartości zerowej wyrażonej w kodzie do modelu EF Core i bazy danych, a także eliminuje użycie interfejsu API Fluent lub adnotacji danych w celu wyrażenia tego samego pojęcia dwa razy.

> [!NOTE]
> Należy zachować ostrożność podczas włączania typów referencyjnych dopuszczających wartość null w istniejącym projekcie: właściwości typu referencyjnego, które zostały wcześniej skonfigurowane jako opcjonalne, zostaną teraz skonfigurowane zgodnie z wymaganiami, chyba że zostaną jawnie dodane do wartości null. W przypadku zarządzania schematem relacyjnej bazy danych może to spowodować wygenerowanie migracji, co pozwala zmienić wartość null kolumny bazy danych.

Aby uzyskać więcej informacji na temat typów referencyjnych dopuszczających wartości null i sposobu ich używania z EF Core, [Zobacz stronę dedykowanej dokumentacji dla tej funkcji](xref:core/miscellaneous/nullable-reference-types).

## <a name="configuration"></a>Konfiguracja

Właściwość, która będzie opcjonalna w Konwencji, można skonfigurować tak, aby była wymagana w następujący sposób:

# <a name="data-annotationstabdata-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
