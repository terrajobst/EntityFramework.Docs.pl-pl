---
title: Śledzenie a zapytania bez śledzenia - EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417650"
---
# <a name="tracking-vs-no-tracking-queries"></a>Śledzenie a zapytania bez śledzenia

Śledzenie zachowanie kontroluje, jeśli Entity Framework Core będzie przechowywać informacje o wystąpieniu jednostki w jego śledzenia zmian. Jeśli jednostka jest śledzona, wszelkie zmiany wykryte w jednostce `SaveChanges()`zostaną utrwalone w bazie danych podczas . EF Core będzie również naprawić właściwości nawigacji między jednostkami w wyniku kwerendy śledzenia i jednostek, które znajdują się w monitorze zmian.

> [!NOTE]
> [Typy jednostek bezkluzywnic](xref:core/modeling/keyless-entity-types) nigdy nie są śledzone. Wszędzie tam, gdzie w tym artykule wspomina typów jednostek, odnosi się do typów jednostek, które mają klucz zdefiniowany.

> [!TIP]  
> Możesz wyświetlić [ten](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) przykład artykułu na GitHub.

## <a name="tracking-queries"></a>Śledzenie zapytań

Domyślnie kwerendy, które zwracają typy jednostek są śledzone. Co oznacza, że można wprowadzać zmiany w tych `SaveChanges()`wystąpieniach encji i utrwalić te zmiany przez . W poniższym przykładzie zmiana klasyfikacji blogów zostanie wykryta i utrwalona w bazie danych podczas `SaveChanges()`programu .

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Zapytania bez śledzenia

Żadne zapytania śledzenia są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu. Są one szybsze do wykonania, ponieważ nie ma potrzeby konfigurowania informacji śledzenia zmian. Jeśli nie trzeba aktualizować jednostek pobranych z bazy danych, należy użyć kwerendy bez śledzenia. Można zamienić poszczególne zapytania, aby nie śledzić.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Można również zmienić domyślne zachowanie śledzenia na poziomie wystąpienia kontekstu:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Rozpoznawanie tożsamości

Ponieważ kwerenda śledząca używa śledzenia zmian, EF Core wykona rozpoznawanie tożsamości w kwerendzie śledzącej. Podczas materializacji jednostki EF Core zwróci to samo wystąpienie jednostki z śledzenia zmian, jeśli jest już śledzone. Jeśli wynik zawiera tę samą jednostkę wiele razy, można uzyskać z powrotem to samo wystąpienie dla każdego wystąpienia. Zapytania bez śledzenia nie używają śledzenia zmian i nie wykonują rozpoznawania tożsamości. Więc można uzyskać z powrotem nowe wystąpienie jednostki, nawet wtedy, gdy ta sama jednostka jest zawarta w wyniku wiele razy. To zachowanie było inne w wersjach przed EF Core 3.0, zobacz [poprzednie wersje](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Śledzenie i projekcje niestandardowe

Nawet jeśli typ wyniku kwerendy nie jest typem jednostki, EF Core będzie nadal śledzić typy jednostek zawarte w wyniku domyślnie. W poniższej kwerendzie, która zwraca typ `Blog` anonimowy, wystąpienia w zestawie wyników będą śledzone.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Jeśli zestaw wyników zawiera typy jednostek wychodzących ze składu LINQ, EF Core będzie je śledzić.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Jeśli zestaw wyników nie zawiera żadnych typów jednostek, nie jest wykonywane śledzenie. W poniższej kwerendzie zwracamy typ anonimowy z niektórymi wartościami z jednostki (ale bez wystąpień rzeczywistego typu jednostki). Nie ma żadnych śledzonych jednostek wychodzących z kwerendy.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core obsługuje wykonywanie oceny klienta w projekcji najwyższego poziomu. Jeśli EF Core materializuje wystąpienie jednostki do oceny klienta, będzie śledzony. W tym miejscu, `blog` ponieważ przekazujemy jednostki do metody `StandardizeURL`klienta, EF Core będzie śledzić wystąpienia blogu zbyt.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core nie śledzi wystąpień jednostek bezkluzynowych zawartych w wyniku. Ale EF Core śledzi wszystkie inne wystąpienia typów jednostek z kluczem zgodnie z powyższymi regułami.

Niektóre z powyższych reguł działały inaczej przed EF Core 3.0. Aby uzyskać więcej informacji, zobacz [poprzednie wersje](#previous-versions).

## <a name="previous-versions"></a>Poprzednie wersje

Przed wersją 3.0 EF Core miał pewne różnice w sposobie śledzenia. Znaczące różnice są następujące:

- Jak wyjaśniono w [client vs Server Evaluation](xref:core/querying/client-eval) strony EF Core obsługiwane oceny klienta w dowolnej części kwerendy przed wersją 3.0. Ocena klienta spowodowała materializację jednostek, które nie były częścią wyniku. Więc EF Core przeanalizował wynik, aby wykryć, co śledzić. Ten projekt miał pewne różnice w następujący sposób:
  - Ocena klienta w projekcji, która spowodowała materializację, ale nie zwróciła zmaterializowane wystąpienie jednostki nie była śledzona. W poniższym przykładzie `blog` nie śledzenie jednostek.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core nie śledzić obiektów pochodzących z składu LINQ w niektórych przypadkach. W poniższym przykładzie `Post`nie było śledzenia .
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Za każdym razem, gdy wyniki kwerendy zawierały typy jednostek bezkluzytowych, cała kwerenda została wykonana bez śledzenia. Oznacza to, że typy jednostek z kluczami, które są w wyniku nie były śledzone albo.
- EF Core nie rozpoznawania tożsamości w kwerendzie nie śledzenia. Użyto słabych odwołań do śledzenia jednostek, które zostały już zwrócone. Więc jeśli zestaw wyników zawiera te same wielokrotności jednostki razy, można uzyskać to samo wystąpienie dla każdego wystąpienia. Chociaż jeśli poprzedni wynik z tej samej tożsamości wyszedł z zakresu i dostał śmieci zebrane, EF Core zwrócił nowe wystąpienie.
