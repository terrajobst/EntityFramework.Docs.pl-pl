---
title: Typy danych — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993524"
---
# <a name="data-types"></a><span data-ttu-id="11a62-102">Typy danych</span><span class="sxs-lookup"><span data-stu-id="11a62-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="11a62-103">Ogólnie rzecz biorąc jest odpowiednie dla relacyjnych baz danych konfiguracji w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="11a62-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="11a62-104">Metody rozszerzenia, pokazane tutaj staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionej *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="11a62-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="11a62-105">Typ danych odnosi się do bazy danych określonego typu kolumny, z którą właściwość jest zamapowana.</span><span class="sxs-lookup"><span data-stu-id="11a62-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="11a62-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="11a62-106">Conventions</span></span>

<span data-ttu-id="11a62-107">Zgodnie z Konwencją dostawcy bazy danych wybiera typ danych, w zależności od typu CLR właściwości.</span><span class="sxs-lookup"><span data-stu-id="11a62-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="11a62-108">On uwzględnia również inne metadane, takie jak skonfigurowanych [maksymalną długość](../max-length.md), czy właściwość jest częścią klucza podstawowego, itp.</span><span class="sxs-lookup"><span data-stu-id="11a62-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="11a62-109">Na przykład program SQL Server używa `datetime2(7)` dla `DateTime` właściwości, a `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` dla `string` właściwości, które są używane jako klucz).</span><span class="sxs-lookup"><span data-stu-id="11a62-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="11a62-110">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="11a62-110">Data Annotations</span></span>

<span data-ttu-id="11a62-111">Korzystanie z adnotacji danych, aby określić dokładny typ danych dla kolumny.</span><span class="sxs-lookup"><span data-stu-id="11a62-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="11a62-112">Na przykład poniższy kod służy do konfigurowania `Url` jako ciąg znaków innego niż unicode o maksymalnej długości `200` i `Rating` jako dziesiętna z dokładnością do `5` i skalowanie z `2`.</span><span class="sxs-lookup"><span data-stu-id="11a62-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="11a62-113">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="11a62-113">Fluent API</span></span>

<span data-ttu-id="11a62-114">Można również określić ten sam typ danych dla kolumn za pomocą Fluent interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="11a62-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
