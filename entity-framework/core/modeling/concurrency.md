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
# <a name="concurrency-tokens"></a><span data-ttu-id="58359-102">Tokeny współbieżności</span><span class="sxs-lookup"><span data-stu-id="58359-102">Concurrency Tokens</span></span>

<span data-ttu-id="58359-103">Jeśli właściwość jest skonfigurowana jako tokenu współbieżności następnie EF sprawdzi żaden inny użytkownik zmodyfikował tę wartość w bazie danych podczas zapisywania zmian w tym rekordzie.</span><span class="sxs-lookup"><span data-stu-id="58359-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="58359-104">EF korzysta ze wzorca optymistycznej współbieżności, czyli będzie przyjęto założenie, że nie zmieniono wartość i spróbuj zapisać dane, ale zgłoszenia, jeśli znajdzie się, że wartość została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="58359-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="58359-105">Na przykład firma Microsoft może być konieczne skonfigurowanie `LastName` na `Person` być tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="58359-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="58359-106">Oznacza to, że jeśli jeden użytkownik próbuje zapisać pewnych zmian `Person`, ale został zmieniony przez innego użytkownika `LastName` , a następnie zostanie wygenerowany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="58359-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="58359-107">Może to być pożądane, dzięki czemu aplikacja może monituje użytkownika o upewnij się, że ten rekord nadal reprezentuje sama osoba rzeczywiste przed zapisaniem zmian.</span><span class="sxs-lookup"><span data-stu-id="58359-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="58359-108">Ta strona dokumenty, jak skonfigurować tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="58359-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="58359-109">Zobacz [Obsługa współbieżności](../saving/concurrency.md) przykłady tego, jak użyć optymistycznej współbieżności dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="58359-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="58359-110">Jak działa tokeny współbieżności w EF</span><span class="sxs-lookup"><span data-stu-id="58359-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="58359-111">Magazyny danych można wymusić tokeny współbieżności, sprawdzając, czy każdy rekord jest zaktualizowane lub usunięte nadal ma taką samą wartość tokenu współbieżności, który został przypisany, gdy pierwotnie kontekstu ładowania danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58359-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="58359-112">Na przykład relacyjnych baz danych to osiągnąć przy wraz z tokenem współbieżności w `WHERE` klauzuli dowolnego `UPDATE` lub `DELETE` polecenia i sprawdzanie liczby wierszy, które miały wpływ.</span><span class="sxs-lookup"><span data-stu-id="58359-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="58359-113">Jeśli token współbieżności nadal odpowiada jeden wiersz zostanie zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="58359-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="58359-114">Jeśli wartość w bazie danych została zmieniona, żadne wiersze zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="58359-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="58359-115">Konwencje</span><span class="sxs-lookup"><span data-stu-id="58359-115">Conventions</span></span>

<span data-ttu-id="58359-116">Według Konwencji właściwości nigdy nie są skonfigurowane jako tokeny współbieżności.</span><span class="sxs-lookup"><span data-stu-id="58359-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="58359-117">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="58359-117">Data Annotations</span></span>

<span data-ttu-id="58359-118">Adnotacje danych służy do konfigurowania właściwości jako tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="58359-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="58359-119">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="58359-119">Fluent API</span></span>

<span data-ttu-id="58359-120">Interfejsu API Fluent umożliwia skonfigurowanie właściwości jako tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="58359-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="58359-121">Wersja wiersza/znacznik czasu</span><span class="sxs-lookup"><span data-stu-id="58359-121">Timestamp/row version</span></span>

<span data-ttu-id="58359-122">Sygnatura czasowa jest właściwością, gdzie nowa wartość jest generowany przez bazę danych, za każdym razem, gdy wiersz jest wstawiane lub aktualizowane.</span><span class="sxs-lookup"><span data-stu-id="58359-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="58359-123">Właściwości są także traktowane jako tokenem współbieżności.</span><span class="sxs-lookup"><span data-stu-id="58359-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="58359-124">Dzięki temu otrzymasz wyjątek, jeśli osobom został zmodyfikowany na wiersz, który próbujesz zaktualizować, ponieważ zapytanie dotyczące danych.</span><span class="sxs-lookup"><span data-stu-id="58359-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="58359-125">Jak jest to osiągane jest używany dostawca bazy danych.</span><span class="sxs-lookup"><span data-stu-id="58359-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="58359-126">Dla programu SQL Server sygnatury czasowej jest zwykle używany w *byte []* właściwość, która będzie można skonfigurować jako *ROWVERSION* kolumny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="58359-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="58359-127">Konwencje</span><span class="sxs-lookup"><span data-stu-id="58359-127">Conventions</span></span>

<span data-ttu-id="58359-128">Według Konwencji właściwości nigdy nie są skonfigurowane jako sygnatur czasowych.</span><span class="sxs-lookup"><span data-stu-id="58359-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="58359-129">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="58359-129">Data Annotations</span></span>

<span data-ttu-id="58359-130">Adnotacje danych służy do konfigurowania właściwości jako sygnaturę czasową.</span><span class="sxs-lookup"><span data-stu-id="58359-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="58359-131">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="58359-131">Fluent API</span></span>

<span data-ttu-id="58359-132">Do skonfigurowania właściwości jako sygnaturę czasową, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="58359-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
