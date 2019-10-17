---
title: Śledzenie i zapytania bez śledzenia — EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 66988f936ab75e17620398c8f21e4a32bbc950bd
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445949"
---
# <a name="tracking-vs-no-tracking-queries"></a>Śledzenie a zapytania bez śledzenia

Śledzenie zachowań, jeśli Entity Framework Core będzie przechowywać informacje o wystąpieniu jednostki w jego monitorze zmian. W przypadku śledzenia jednostki wszystkie zmiany wykryte w jednostce zostaną utrwalone w bazie danych podczas `SaveChanges()`. EF Core również poprawi właściwości nawigacji między jednostkami w wyniku zapytania śledzenia i jednostkami, które znajdują się w module śledzącym zmiany.

> [!NOTE]
> [Typy jednostek](xref:core/modeling/keyless-entity-types) nie są nigdy śledzone. Gdziekolwiek w tym artykule opisano typy jednostek, odwołują się do typów jednostek, które mają zdefiniowany klucz.

> [!TIP]  
> [Przykład](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) tego artykułu można wyświetlić w witrynie GitHub.

## <a name="tracking-queries"></a>Zapytania śledzenia

Domyślnie zapytania zwracające typy jednostek są śledzone. Oznacza to, że można wprowadzać zmiany w tych wystąpieniach jednostek i mieć te zmiany utrwalane przez `SaveChanges()`. W poniższym przykładzie zmiana klasyfikacji blogów zostanie wykryta i utrwalona w bazie danych podczas `SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Zapytania bez śledzenia

Zapytania śledzenia nie są przydatne, gdy wyniki są używane w scenariuszu tylko do odczytu. Są one szybsze do wykonania, ponieważ nie ma potrzeby konfigurowania informacji o śledzeniu zmian. Jeśli nie musisz aktualizować jednostek pobranych z bazy danych, należy użyć zapytania bez śledzenia. Pojedyncze zapytanie można zamienić na nie śledzenie.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Możesz również zmienić domyślne zachowanie śledzenia na poziomie wystąpienia kontekstu:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Rozpoznawanie tożsamości

Ponieważ zapytanie śledzenia używa śledzenia zmian, EF Core przeprowadzi rozpoznawanie tożsamości w zapytaniu śledzenia. Gdy materializacji jednostkę, EF Core zwróci to samo wystąpienie jednostki z narzędzia do śledzenia zmian, jeśli jest już śledzone. Jeśli wynik zawiera tę samą jednostkę wiele razy, powracasz do tego samego wystąpienia dla każdego wystąpienia. Zapytania bez śledzenia nie używają śledzenia zmian i nie obsługują rozpoznawania tożsamości. Należy więc odzyskać nowe wystąpienie jednostki, nawet gdy ta sama jednostka jest zawarta w wyniku wiele razy. To zachowanie różni się w wersjach przed EF Core 3,0, zobacz [poprzednie wersje](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Śledzenie i projekcje niestandardowe

Nawet jeśli typ wyniku zapytania nie jest typem jednostki, EF Core nadal będzie śledzić typy jednostek zawarte w wyniku. W poniższym zapytaniu, które zwraca typ anonimowy, będą śledzone wystąpienia `Blog` w zestawie wyników.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Jeśli zestaw wyników zawiera typy jednostek wychodzące z kompozycji LINQ, EF Core będą je śledzić.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Jeśli zestaw wyników nie zawiera żadnych typów jednostek, śledzenie nie jest wykonywane. W poniższym zapytaniu zwracamy typ anonimowy z niektórymi wartościami z jednostki (ale nie wystąpienia rzeczywistego typu jednostki). Brak śledzonych jednostek wychodzących z zapytania.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core obsługuje Obliczanie klienta w projekcji najwyższego poziomu. Jeśli EF Core materializuje wystąpienie jednostki na potrzeby oceny klienta, zostanie ono śledzone. W tym miejscu, ponieważ przekazujemy jednostki `blog` do metody klienta `StandardizeURL`, EF Core śledzi również wystąpienia blogu.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core nie śledzi wystąpień jednostek o niższym stopniu zawartych w wyniku. Ale EF Core śledzi wszystkie inne wystąpienia typów jednostek z kluczem według powyższych reguł.

Niektóre z powyższych reguł działały inaczej niż EF Core 3,0. Aby uzyskać więcej informacji, zobacz [poprzednie wersje](#previous-versions).

## <a name="previous-versions"></a>Poprzednie wersje

Przed wersjami 3,0 EF Core miały pewne różnice w sposobie działania śledzenia. Istotne różnice są następujące:

- Zgodnie z objaśnieniem na stronie [oceny klienta programu vs Server](xref:core/querying/client-eval) EF Core obsługiwane oceny klienta w dowolnej części zapytania przed wersją 3,0. Obliczanie klienta spowodowało materializację jednostek, które nie były częścią wyniku. Dlatego EF Core analizować wynik, aby wykryć, co należy śledzić. Ten projekt miał pewne różnice w następujący sposób:
  - Ocena klienta w projekcji, która spowodowała materializację, ale nie zwróciła wystąpienia jednostki, która nie została prześledzona. Poniższy przykład nie śledzi jednostek `blog`.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core nie śledzi w niektórych przypadkach obiektów wychodzących ze składu LINQ. Poniższy przykład nie śledzi `Post`.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Za każdym razem, gdy wyniki zapytania zawierają typy jednostek bez typu, całe zapytanie zostało wykonane jako Nieśledzone. Oznacza to, że typy jednostek z kluczami, które w wyniku nie są śledzone.
- EF Coreł rozpoznawanie tożsamości w zapytaniu bez śledzenia. Używa on słabych odwołań do śledzenia jednostek, które zostały już zwrócone. W związku z tym, jeśli zestaw wyników zawiera te same czasy wielokrotności, należy uzyskać to samo wystąpienie dla każdego wystąpienia. Mimo że poprzedni wynik z tą samą tożsamością wystąpił poza zakresem i pobrano elementy bezużyteczne, EF Core zwróciło nowe wystąpienie.
