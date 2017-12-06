---
title: "Typy danych — podstawowe EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a><span data-ttu-id="1fb9e-102">Typy danych</span><span class="sxs-lookup"><span data-stu-id="1fb9e-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="1fb9e-103">Konfiguracja opisana w tej sekcji ma zastosowanie do relacyjnych baz danych w zasadzie.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1fb9e-104">Metody rozszerzenia pokazane staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (z powodu udostępnionego *Microsoft.EntityFrameworkCore.Relational* pakietu).</span><span class="sxs-lookup"><span data-stu-id="1fb9e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1fb9e-105">Typ danych odwołuje się do bazy danych określonego typu kolumny, z którą właściwość jest zamapowana.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="1fb9e-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="1fb9e-106">Conventions</span></span>

<span data-ttu-id="1fb9e-107">Według Konwencji dostawcy bazy danych wybiera typ danych na podstawie typu CLR właściwości.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="1fb9e-108">Uwzględnia ona, również inne metadane, takie jak skonfigurowanego [maksymalną długość](../max-length.md), czy właściwość jest częścią klucza podstawowego, itd.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="1fb9e-109">Na przykład program SQL Server używa `datetime2(7)` dla `DateTime` właściwości, oraz `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` dla `string` właściwości, które są używane jako klucz).</span><span class="sxs-lookup"><span data-stu-id="1fb9e-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1fb9e-110">Adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="1fb9e-110">Data Annotations</span></span>

<span data-ttu-id="1fb9e-111">Adnotacje danych służy do określenia typu dokładne dane dla kolumny.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="1fb9e-112">Na przykład następujący kod konfiguruje `Url` jako ciąg z systemem innym niż unicode o maksymalnej długości `200` i `Rating` dziesiętnego z dokładnością do `5` i skalować z `2`.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="1fb9e-113">Interfejsu API Fluent</span><span class="sxs-lookup"><span data-stu-id="1fb9e-113">Fluent API</span></span>

<span data-ttu-id="1fb9e-114">Aby określić ten sam typ danych dla kolumny można również Użyj interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="1fb9e-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
