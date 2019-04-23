---
title: Maksymalna długość numeru PIN — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929852"
---
# <a name="maximum-length"></a><span data-ttu-id="61c89-102">Maksymalna długość</span><span class="sxs-lookup"><span data-stu-id="61c89-102">Maximum Length</span></span>

<span data-ttu-id="61c89-103">Konfigurowanie maksymalnej długości stanowi wskazówkę z magazynem danych o typie danych odpowiednich dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="61c89-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="61c89-104">Maksymalna długość ma zastosowanie tylko do tablicy typów danych, takich jak `string` i `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="61c89-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="61c89-105">Platformy Entity Framework wykonaj wszelkie weryfikacji o maksymalnej długości przed przekazaniem do dostawcy.</span><span class="sxs-lookup"><span data-stu-id="61c89-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="61c89-106">Jest magazynu dostawcy lub danych, sprawdź, czy jest to odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="61c89-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="61c89-107">Na przykład gdy przeznaczonych dla programu SQL Server, która przekracza maksymalną długość spowodują wyjątek jako typ danych kolumny źródłowej nie zezwoli nadmiarowych danych mają być przechowywane.</span><span class="sxs-lookup"><span data-stu-id="61c89-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="61c89-108">Konwencje</span><span class="sxs-lookup"><span data-stu-id="61c89-108">Conventions</span></span>

<span data-ttu-id="61c89-109">Zgodnie z Konwencją pozostało do dostawcy bazy danych, aby wybrać odpowiedni typ danych właściwości.</span><span class="sxs-lookup"><span data-stu-id="61c89-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="61c89-110">Dla właściwości o długości dostawca bazy danych zazwyczaj wybierz typ danych, który umożliwia najdłuższy długość danych.</span><span class="sxs-lookup"><span data-stu-id="61c89-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="61c89-111">Na przykład Microsoft SQL Server będzie używać `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` Jeśli kolumna jest używana jako klucz).</span><span class="sxs-lookup"><span data-stu-id="61c89-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="61c89-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="61c89-112">Data Annotations</span></span>

<span data-ttu-id="61c89-113">Korzystanie z adnotacji danych, aby skonfigurować maksymalną długość dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="61c89-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="61c89-114">W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` typ danych jest używany.</span><span class="sxs-lookup"><span data-stu-id="61c89-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="61c89-115">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="61c89-115">Fluent API</span></span>

<span data-ttu-id="61c89-116">Interfejs Fluent API umożliwiają skonfigurowanie maksymalnej długości dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="61c89-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="61c89-117">W tym przykładzie przeznaczonych dla programu SQL Server, spowodowałoby `nvarchar(500)` typ danych jest używany.</span><span class="sxs-lookup"><span data-stu-id="61c89-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
