---
title: Wstępne wypełnianie danych — EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994482"
---
# <a name="data-seeding"></a>Wstępne wypełnianie danych

> [!NOTE]  
> Ta funkcja jest nowa na platformie EF Core 2.1.

Wstępne wypełnianie danych umożliwia zapewnienie początkowe dane, aby wypełnić bazę danych. W odróżnieniu od w EF6 w programie EF Core wstępne wypełnianie danych jest skojarzony z typem jednostki jako część konfiguracji modelu. Następnie programu EF Core [migracje](xref:core/managing-schemas/migrations/index) można automatycznie obliczyć co Wstawianie, aktualizowanie lub usuwanie potrzebę operacji mają być stosowane podczas uaktualniania bazy danych do nowej wersji modelu.

Na przykład można to skonfigurować dane `Blog` w `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Aby dodać jednostki, które mają relację wartości klucza obcego muszą być określone. Często właściwości klucza obcego są w stanie w tle, tak aby można było ustawić wartości Klasa anonimowa powinny być używane:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Po dodaniu jednostki, zaleca się używanie [migracje](xref:core/managing-schemas/migrations/index) Aby zastosować zmiany. 

Alternatywnie, można użyć `context.Database.EnsureCreated()` do utworzenia nowej bazy danych zawierającej dane inicjatora, na przykład w przypadku bazy danych testów lub przy użyciu dostawcy w pamięci. Należy pamiętać, że jeśli baza danych już istnieje, `EnsureCreated()` spowoduje zaktualizowanie żadnego schematu ani w bazie danych inicjatora.
