---
title: Sekwencje — EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417441"
---
# <a name="sequences"></a>Sekwencje

> [!NOTE]  
> Sekwencje to funkcja zwykle obsługiwana tylko przez relacyjne bazy danych. Jeśli używasz nierelacyjnej bazy danych, takiej jak Cosmos, zapoznaj się z dokumentacją bazy danych w celu wygenerowania unikatowych wartości.

Sekwencja generuje unikatowe, sekwencyjne wartości liczbowe w bazie danych. Sekwencje nie są skojarzone z określoną tabelą i można skonfigurować wiele tabel, aby rysować wartości z jednej sekwencji.

## <a name="basic-usage"></a>Podstawowe użycie

Można skonfigurować sekwencję w modelu, a następnie użyć jej do generowania wartości właściwości:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

Należy zauważyć, że określony kod SQL używany do generowania wartości z sekwencji jest specyficzny dla bazy danych; Powyższy przykład działa na SQL Server, ale zakończy się niepowodzeniem w innych bazach danych. Aby uzyskać więcej informacji, zapoznaj się z dokumentacją konkretnej bazy danych.

## <a name="configuring-sequence-settings"></a>Konfigurowanie ustawień sekwencji

Można również skonfigurować dodatkowe aspekty sekwencji, takie jak jej schemat, wartość początkowa, przyrost itp.:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
