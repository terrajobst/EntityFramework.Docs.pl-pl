---
title: "Tokeny współbieżności - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Tokeny współbieżności

Jeśli właściwość jest skonfigurowana jako tokenu współbieżności następnie EF sprawdzi żaden inny użytkownik zmodyfikował tę wartość w bazie danych podczas zapisywania zmian w tym rekordzie. EF korzysta ze wzorca optymistycznej współbieżności, czyli będzie przyjęto założenie, że nie zmieniono wartość i spróbuj zapisać dane, ale zgłoszenia, jeśli znajdzie się, że wartość została zmieniona.

Na przykład firma Microsoft może być konieczne skonfigurowanie `LastName` na `Person` być tokenem współbieżności. Oznacza to, że jeśli jeden użytkownik próbuje zapisać pewnych zmian `Person`, ale został zmieniony przez innego użytkownika `LastName` , a następnie zostanie wygenerowany wyjątek. Może to być pożądane, dzięki czemu aplikacja może monituje użytkownika o upewnij się, że ten rekord nadal reprezentuje sama osoba rzeczywiste przed zapisaniem zmian.

> [!NOTE]
> Ta strona dokumenty, jak skonfigurować tokeny współbieżności. Zobacz [Obsługa współbieżności](../saving/concurrency.md) przykłady tego, jak użyć optymistycznej współbieżności dla aplikacji.

## <a name="how-concurrency-tokens-work-in-ef"></a>Jak działa tokeny współbieżności w EF

Magazyny danych można wymusić tokeny współbieżności, sprawdzając, czy każdy rekord jest zaktualizowane lub usunięte nadal ma taką samą wartość tokenu współbieżności, który został przypisany, gdy pierwotnie kontekstu ładowania danych z bazy danych.

Na przykład relacyjnych baz danych to osiągnąć przy wraz z tokenem współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` polecenia i sprawdzanie liczby wierszy, które miały wpływ. Jeśli token współbieżności nadal odpowiada jeden wiersz zostanie zaktualizowany. Jeśli wartość w bazie danych została zmieniona, żadne wiersze zostały zaktualizowane.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>Konwencje

Według Konwencji właściwości nigdy nie są skonfigurowane jako tokeny współbieżności.

## <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do konfigurowania właściwości jako tokenem współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Interfejsu API Fluent

Interfejsu API Fluent umożliwia skonfigurowanie właściwości jako tokenem współbieżności.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Wersja wiersza/znacznik czasu

Sygnatura czasowa jest właściwością, gdzie nowa wartość jest generowany przez bazę danych, za każdym razem, gdy wiersz jest wstawiane lub aktualizowane. Właściwości są także traktowane jako tokenem współbieżności. Dzięki temu otrzymasz wyjątek, jeśli osobom został zmodyfikowany na wiersz, który próbujesz zaktualizować, ponieważ zapytanie dotyczące danych.

Jak jest to osiągane jest używany dostawca bazy danych. Dla programu SQL Server sygnatury czasowej jest zwykle używany w *byte []* właściwość, która będzie można skonfigurować jako *ROWVERSION* kolumny w bazie danych.

### <a name="conventions"></a>Konwencje

Według Konwencji właściwości nigdy nie są skonfigurowane jako sygnatur czasowych.

### <a name="data-annotations"></a>Adnotacji danych

Adnotacje danych służy do konfigurowania właściwości jako sygnaturę czasową.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Interfejsu API Fluent

Do skonfigurowania właściwości jako sygnaturę czasową, można użyć interfejsu API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
