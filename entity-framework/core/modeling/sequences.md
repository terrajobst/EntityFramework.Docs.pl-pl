---
title: Sekwencje — EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502482"
---
# <a name="sequences"></a><span data-ttu-id="c4ee1-102">Sekwencje</span><span class="sxs-lookup"><span data-stu-id="c4ee1-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="c4ee1-103">Sekwencje to funkcja zwykle obsługiwana tylko przez relacyjne bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c4ee1-103">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="c4ee1-104">Jeśli używasz nierelacyjnej bazy danych, takiej jak Cosmos, zapoznaj się z dokumentacją bazy danych w celu wygenerowania unikatowych wartości.</span><span class="sxs-lookup"><span data-stu-id="c4ee1-104">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="c4ee1-105">Sekwencja generuje unikatowe, sekwencyjne wartości liczbowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="c4ee1-105">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="c4ee1-106">Sekwencje nie są skojarzone z określoną tabelą i można skonfigurować wiele tabel, aby rysować wartości z jednej sekwencji.</span><span class="sxs-lookup"><span data-stu-id="c4ee1-106">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="c4ee1-107">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="c4ee1-107">Basic usage</span></span>

<span data-ttu-id="c4ee1-108">Można skonfigurować sekwencję w modelu, a następnie użyć jej do generowania wartości właściwości:</span><span class="sxs-lookup"><span data-stu-id="c4ee1-108">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="c4ee1-109">Należy zauważyć, że określony kod SQL używany do generowania wartości z sekwencji jest specyficzny dla bazy danych; Powyższy przykład działa na SQL Server, ale zakończy się niepowodzeniem w innych bazach danych.</span><span class="sxs-lookup"><span data-stu-id="c4ee1-109">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="c4ee1-110">Aby uzyskać więcej informacji, zapoznaj się z dokumentacją konkretnej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c4ee1-110">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="c4ee1-111">Konfigurowanie ustawień sekwencji</span><span class="sxs-lookup"><span data-stu-id="c4ee1-111">Configuring sequence settings</span></span>

<span data-ttu-id="c4ee1-112">Można również skonfigurować dodatkowe aspekty sekwencji, takie jak jej schemat, wartość początkowa, przyrost itp.:</span><span class="sxs-lookup"><span data-stu-id="c4ee1-112">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
