---
title: Dziedziczenie — EF Core
description: Jak skonfigurować dziedziczenie typu jednostki przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502174"
---
# <a name="inheritance"></a>Dziedziczenie

EF można zmapować hierarchię typów .NET do bazy danych. Dzięki temu można pisać jednostki platformy .NET w kodzie w zwykły sposób, przy użyciu podstawowych i pochodnych typów, a program EF bezproblemowo tworzy odpowiedni schemat bazy danych, wysyła zapytania itp. Rzeczywiste szczegóły dotyczące sposobu mapowania hierarchii typów są zależne od dostawcy. Ta strona zawiera opis obsługi dziedziczenia w kontekście relacyjnej bazy danych.

W tej chwili EF Core obsługuje tylko wzorzec Table-per-Hierarchy (TPH). TPH używa pojedynczej tabeli do przechowywania danych dla wszystkich typów w hierarchii, a kolumna rozróżniacza służy do identyfikowania, który typ reprezentuje każdy wiersz.

> [!NOTE]
> EF Core nie są jeszcze obsługiwane przez program na podstawie typu tabeli (TPT) i typu tabeli (TPC), które są obsługiwane przez EF6. TPT to główna funkcja zaplanowana dla EF Core 5,0.

## <a name="entity-type-hierarchy-mapping"></a>Mapowanie hierarchii typów jednostek

Zgodnie z Konwencją EF skonfiguruje tylko dziedziczenie, jeśli dwa lub więcej dziedziczonych typów jest jawnie uwzględnionych w modelu. EF nie będzie automatycznie skanować dla typów podstawowych lub pochodnych, które nie są uwzględnione w modelu.

Można uwzględnić typy w modelu, uwidaczniając Nieogólnymi dla każdego typu w hierarchii dziedziczenia:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

Ten model jest mapowany na następujący schemat bazy danych (należy zwrócić uwagę na niejawnie utworzoną kolumnę *rozróżniacza* , która określa, który typ *blogu* jest przechowywany w każdym wierszu):

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> W przypadku korzystania z mapowania TPH kolumny bazy danych są automatycznie wprowadzane do wartości null. Na przykład kolumna *RssUrl* ma wartość null, ponieważ zwykłe wystąpienia *blogu* nie mają tej właściwości.

Jeśli nie chcesz uwidaczniać Nieogólnymi dla co najmniej jednej jednostki w hierarchii, możesz również użyć interfejsu API Fluent, aby upewnić się, że są one uwzględnione w modelu.

> [!TIP]
> Jeśli nie korzystasz z Konwencji, możesz określić typ podstawowy jawnie przy użyciu `HasBaseType`. Możesz również użyć `.HasBaseType((Type)null)`, aby usunąć typ jednostki z hierarchii.

## <a name="discriminator-configuration"></a>Konfiguracja rozróżniacza

Można skonfigurować nazwę i typ kolumny rozróżniacza oraz wartości, które są używane do identyfikacji poszczególnych typów w hierarchii:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

W powyższych przykładach EF dodał rozróżniacz niejawnie jako [Właściwość Shadow](xref:core/modeling/shadow-properties) w jednostce podstawowej hierarchii. Tę właściwość można skonfigurować w taki sam sposób:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

Na koniec rozróżniacz może być również mapowany na zwykłą Właściwość platformy .NET w jednostce:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a>Udostępnione kolumny

Domyślnie, gdy dwa typy jednostek równorzędnych w hierarchii mają właściwość o tej samej nazwie, zostaną zamapowane na dwie oddzielne kolumny. Jeśli jednak ich typ jest identyczny, można go zamapować na tę samą kolumnę bazy danych:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
