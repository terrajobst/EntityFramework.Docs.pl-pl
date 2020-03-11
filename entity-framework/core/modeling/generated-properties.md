---
title: Wygenerowane wartości — EF Core
description: Jak skonfigurować generowanie wartości dla właściwości przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416347"
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

## <a name="value-generated-on-add"></a>Wartość wygenerowana przy dodawaniu

Zgodnie z Konwencją, niezłożone klucze podstawowe typu short, int, Long lub GUID są skonfigurowane tak, aby wartości wygenerowały dla wstawionych jednostek, jeśli wartość nie jest dostarczana przez aplikację. Dostawca bazy danych zwykle zajmuje się wymaganą konfiguracją; na przykład numeryczny klucz podstawowy w SQL Server jest automatycznie ustawiany jako kolumna tożsamości.

Można skonfigurować dowolną właściwość, aby wygenerowała wartość dla wstawionych jednostek w następujący sposób:

### <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> Pozwala to na to, że EF wie, że wartości są generowane dla dodanych jednostek, nie gwarantuje to, że EF skonfiguruje faktyczny mechanizm generowania wartości. Aby uzyskać więcej informacji, zobacz [wartość wygenerowaną w sekcji Dodaj](#value-generated-on-add) .

### <a name="default-values"></a>Wartości domyślne

W relacyjnych bazach danych kolumna może być skonfigurowana z wartością domyślną; Jeśli wiersz zostanie wstawiony bez wartości dla tej kolumny, zostanie użyta wartość domyślna.

Można skonfigurować wartość domyślną właściwości:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

Można również określić fragment SQL, który jest używany do obliczania wartości domyślnej:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

Określenie wartości domyślnej spowoduje niejawne skonfigurowanie właściwości jako wartości wygenerowanej przy dodawaniu.

## <a name="value-generated-on-add-or-update"></a>Wartość wygenerowana podczas dodawania lub aktualizowania

### <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> Pozwala to na to, że EF wie, że wartości są generowane dla dodanych lub zaktualizowanych jednostek, nie gwarantuje to, że EF skonfiguruje faktyczny mechanizm generowania wartości. Aby uzyskać więcej informacji [, zobacz wartość wygenerowaną w sekcji Dodawanie lub aktualizowanie](#value-generated-on-add-or-update) .

### <a name="computed-columns"></a>Kolumny obliczane

W przypadku niektórych relacyjnych baz danych kolumna może być skonfigurowana do obliczania wartości w bazie danych, zwykle z wyrażeniem odwołującym się do innych kolumn:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> W niektórych przypadkach wartość kolumny jest obliczana za każdym razem, gdy jest ona pobierana (nazywanej czasem kolumnami *wirtualnymi* ), a w innych, jest obliczana na każdej aktualizacji wiersza i przechowywany (czasami nazywanej kolumnami *przechowywanymi* lub *utrwalonymi* ). Różnią się one między dostawcami baz danych.

## <a name="no-value-generation"></a>Brak generowania wartości

Wyłączenie generowania wartości dla właściwości jest zwykle konieczne, jeśli Konwencja skonfiguruje ją do generowania wartości. Na przykład jeśli masz klucz podstawowy typu int, zostanie on niejawnie ustawiony jako wartość wygenerowana podczas dodawania; można ją wyłączyć, wykonując następujące czynności:

### <a name="data-annotations"></a>[Adnotacje danych](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-api"></a>[Interfejs API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
