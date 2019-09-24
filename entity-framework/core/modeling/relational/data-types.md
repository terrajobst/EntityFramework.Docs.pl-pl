---
title: Typy danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197180"
---
# <a name="data-types"></a><span data-ttu-id="76981-102">Typy danych</span><span class="sxs-lookup"><span data-stu-id="76981-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="76981-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="76981-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="76981-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="76981-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="76981-105">Typ danych odnosi się do typu określonego dla bazy danych kolumny, do której jest zamapowana właściwość.</span><span class="sxs-lookup"><span data-stu-id="76981-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="76981-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="76981-106">Conventions</span></span>

<span data-ttu-id="76981-107">Według Konwencji dostawca bazy danych wybiera typ danych na podstawie typu .NET właściwości.</span><span class="sxs-lookup"><span data-stu-id="76981-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="76981-108">Uwzględnia również inne metadane, takie jak skonfigurowana [Maksymalna długość](../max-length.md), czy właściwość jest częścią klucza podstawowego itd.</span><span class="sxs-lookup"><span data-stu-id="76981-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="76981-109">Na przykład SQL Server używa `datetime2(7)` dla `DateTime` właściwości i `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` dla `string` właściwości, które są używane jako klucz).</span><span class="sxs-lookup"><span data-stu-id="76981-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="76981-110">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="76981-110">Data Annotations</span></span>

<span data-ttu-id="76981-111">Możesz użyć adnotacji danych, aby określić dokładny typ danych dla kolumny.</span><span class="sxs-lookup"><span data-stu-id="76981-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="76981-112">Na przykład poniższy kod konfiguruje `Url` jako ciąg niebędący znakiem Unicode z maksymalną `200` długością i `Rating` `2`jako dziesiętną `5` z dokładnością i skalą.</span><span class="sxs-lookup"><span data-stu-id="76981-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="76981-113">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="76981-113">Fluent API</span></span>

<span data-ttu-id="76981-114">Możesz również użyć interfejsu API Fluent, aby określić te same typy danych dla kolumn.</span><span class="sxs-lookup"><span data-stu-id="76981-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
