---
title: Wygenerowane wartości — EF Core
description: Jak skonfigurować generowanie wartości dla właściwości przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 7fa3eae5e2edb7b4c40ed4f99ce4a29f367e622a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824697"
---
# <a name="generated-values"></a>Generowane wartości

## <a name="value-generation-patterns"></a>Wzorce generowania wartości

Istnieją trzy wzorce generowania wartości, których można użyć do właściwości:

* Brak generowania wartości
* Wartość wygenerowana przy dodawaniu
* Wartość wygenerowana podczas dodawania lub aktualizowania

### <a name="no-value-generation"></a>Brak generowania wartości

Generowanie wartości nie oznacza, że zawsze będzie podasz prawidłową wartość, która zostanie zapisana w bazie danych. Ta prawidłowa wartość musi być przypisana do nowych jednostek przed dodaniem ich do kontekstu.

### <a name="value-generated-on-add"></a>Wartość wygenerowana przy dodawaniu

Wartość wygenerowana przy dodawaniu oznacza, że wartość jest generowana dla nowych jednostek.

W zależności od używanego dostawcy bazy danych wartości mogą być generowane po stronie klienta przez EF lub w bazie danych programu. Jeśli wartość jest generowana przez bazę danych, EF może przypisać wartość tymczasową podczas dodawania jednostki do kontekstu. Ta wartość tymczasowa zostanie następnie zastąpiona wartością wygenerowaną przez bazę danych podczas `SaveChanges()`.

W przypadku dodania jednostki do kontekstu, który ma wartość przypisaną do właściwości, EF spróbuje wstawić tę wartość zamiast generować nową. Właściwość jest uznawana za przypisaną wartości, jeśli nie ma przypisanej wartości domyślnej środowiska CLR (`null` dla `string`, `0` dla `int`, `Guid.Empty` dla `Guid`itd.). Aby uzyskać więcej informacji, zobacz [jawne wartości dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Sposób generowania wartości dla dodanych jednostek będzie zależeć od używanego dostawcy bazy danych. Dostawcy bazy danych mogą automatycznie skonfigurować generowanie wartości dla niektórych typów właściwości, ale inne mogą wymagać ręcznej konfiguracji sposobu generowania wartości.
>
> Na przykład podczas korzystania z SQL Server wartości będą generowane automatycznie dla `GUID` właściwości (przy użyciu algorytmu sekwencyjnego identyfikatora GUID SQL Server). Jeśli jednak określisz, że właściwość `DateTime` jest generowana przy dodawaniu, należy skonfigurować sposób generowania wartości. Aby to zrobić, należy skonfigurować wartość domyślną `GETDATE()`, zobacz [wartości domyślne](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Wartość wygenerowana podczas dodawania lub aktualizowania

Wartość wygenerowana podczas dodawania lub aktualizowania oznacza, że nowa wartość jest generowana za każdym razem, gdy rekord jest zapisywany (INSERT lub Update).

Podobnie jak `value generated on add`, jeśli określisz wartość właściwości dla nowo dodanego wystąpienia jednostki, ta wartość zostanie wstawiona, a nie wygenerowana wartość. Istnieje również możliwość ustawienia wartości jawnej podczas aktualizowania. Aby uzyskać więcej informacji, zobacz [jawne wartości dla wygenerowanych właściwości](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Sposób generowania wartości dla dodanych i zaktualizowanych jednostek będzie zależeć od używanego dostawcy bazy danych. Dostawcy bazy danych mogą automatycznie skonfigurować generowanie wartości dla niektórych typów właściwości, podczas gdy inne będą wymagały ręcznej konfiguracji sposobu generowania wartości.
>
> Na przykład podczas korzystania z SQL Server, `byte[]` właściwości, które są ustawione jako generowane przy dodawaniu lub aktualizacji i oznaczone jako tokeny współbieżności, zostaną skonfigurowane z typem danych `rowversion`, dzięki czemu wartości zostaną wygenerowane w bazie danych. Jeśli jednak określisz, że właściwość `DateTime` jest generowana przy dodawaniu lub aktualizacji, musisz skonfigurować sposób generowania wartości. W tym celu należy skonfigurować wartość domyślną `GETDATE()` (zobacz [wartości domyślne](relational/default-values.md)), aby generować wartości dla nowych wierszy. Następnie można użyć wyzwalacza bazy danych do generowania wartości podczas aktualizacji (takich jak Poniższy przykładowy wyzwalacz).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konwencje

Domyślnie niezłożone klucze podstawowe typu short, int, Long lub GUID zostaną skonfigurowane w celu uzyskania wartości generowanych podczas dodawania. Wszystkie inne właściwości zostaną skonfigurowane bez generowania wartości.

## <a name="data-annotations"></a>Adnotacje danych

### <a name="no-value-generation-data-annotations"></a>Nie generowanie wartości (adnotacje danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Wartość wygenerowana przy dodawaniu (adnotacje danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Pozwala to na to, że EF wie, że wartości są generowane dla dodanych jednostek, nie gwarantuje to, że EF skonfiguruje faktyczny mechanizm generowania wartości. Aby uzyskać więcej informacji, zobacz [wartość wygenerowaną w sekcji Dodaj](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-data-annotations"></a>Wartość wygenerowana podczas dodawania lub aktualizowania (adnotacje danych)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Pozwala to na to, że EF wie, że wartości są generowane dla dodanych lub zaktualizowanych jednostek, nie gwarantuje to, że EF skonfiguruje faktyczny mechanizm generowania wartości. Aby uzyskać więcej informacji [, zobacz wartość wygenerowaną w sekcji Dodawanie lub aktualizowanie](#value-generated-on-add-or-update) .

## <a name="fluent-api"></a>Interfejs API Fluent

Za pomocą interfejsu API Fluent można zmienić wzorzec generowania wartości dla danej właściwości.

### <a name="no-value-generation-fluent-api"></a>Brak generowania wartości (interfejs API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Wartość wygenerowana przy dodawaniu (interfejs API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` tylko umożliwia Dr wie, że wartości są generowane dla dodanych jednostek, nie gwarantuje to, że EF skonfiguruje faktyczny mechanizm generowania wartości.  Aby uzyskać więcej informacji, zobacz [wartość wygenerowaną w sekcji Dodaj](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Wartość wygenerowana podczas dodawania lub aktualizowania (interfejs API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Pozwala to na to, że EF wie, że wartości są generowane dla dodanych lub zaktualizowanych jednostek, nie gwarantuje to, że EF skonfiguruje faktyczny mechanizm generowania wartości. Aby uzyskać więcej informacji [, zobacz wartość wygenerowaną w sekcji Dodawanie lub aktualizowanie](#value-generated-on-add-or-update) .
