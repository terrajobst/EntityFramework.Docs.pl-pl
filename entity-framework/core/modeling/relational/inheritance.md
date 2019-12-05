---
title: Dziedziczenie (relacyjna baza danych) — EF Core
description: Jak skonfigurować dziedziczenie typu jednostki w relacyjnej bazie danych przy użyciu Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824744"
---
# <a name="inheritance-relational-database"></a>Dziedziczenie (relacyjna baza danych)

> [!NOTE]  
> Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych. Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).

Dziedziczenie w modelu EF służy do kontrolowania sposobu reprezentowania dziedziczenia w klasach obiektów w bazie danych.

> [!NOTE]  
> Obecnie tylko wzorzec tabeli dla hierarchii (TPH) jest implementowany w EF Core. Inne typowe wzorce, takie jak Table-per-Type (TPT) i typu tabeli (TPC), nie są jeszcze dostępne.

## <a name="conventions"></a>Konwencje

Domyślnie dziedziczenie zostanie zmapowane przy użyciu wzorca tabela-na-hierarchia (TPH). TPH używa pojedynczej tabeli do przechowywania danych dla wszystkich typów w hierarchii. Kolumna rozróżniacza służy do identyfikowania, który typ reprezentuje każdy wiersz.

EF Core zostanie skonfigurowana tylko dziedziczenie, jeśli co najmniej dwa dziedziczone typy zostaną jawnie dołączone do modelu (zobacz [dziedziczenie](../inheritance.md) , aby uzyskać więcej informacji).

Poniżej znajduje się przykład przedstawiający prosty scenariusz dziedziczenia oraz dane przechowywane w tabeli relacyjnej bazy danych przy użyciu wzorca TPH. Kolumna *rozróżniacza* określa, który typ *blogu* jest przechowywany w każdym wierszu.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![obraz](_static/inheritance-tph-data.png)

>[!NOTE]
> W przypadku korzystania z mapowania TPH kolumny bazy danych są automatycznie wprowadzane do wartości null.

## <a name="data-annotations"></a>Adnotacje danych

Nie można użyć adnotacji danych do skonfigurowania dziedziczenia.

## <a name="fluent-api"></a>Interfejs API Fluent

Korzystając z interfejsu API Fluent, można skonfigurować nazwę i typ kolumny dyskryminatora oraz wartości, które są używane do identyfikowania poszczególnych typów w hierarchii.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Konfigurowanie właściwości rozróżniacza

W powyższych przykładach rozróżniacz jest tworzony jako [Właściwość Shadow](xref:core/modeling/shadow-properties) w jednostce podstawowej hierarchii. Ponieważ jest to właściwość w modelu, można ją skonfigurować tak samo jak inne właściwości. Na przykład, aby ustawić maksymalną długość w przypadku użycia domyślnego rozróżniacza Konwencji przez Konwencję:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

Rozróżniacz można także zamapować na Właściwość platformy .NET w jednostce i skonfigurować ją. Na przykład:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Udostępnione kolumny

Gdy dwa typy jednostek równorzędnych mają właściwość o tej samej nazwie, zostaną one domyślnie zmapowane do dwóch oddzielnych kolumn. Ale jeśli są zgodne, można je zamapować na tę samą kolumnę:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]