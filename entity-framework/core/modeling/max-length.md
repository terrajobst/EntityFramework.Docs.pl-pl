---
title: Maksymalna długość — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197225"
---
# <a name="maximum-length"></a><span data-ttu-id="c58aa-102">Maksymalna długość</span><span class="sxs-lookup"><span data-stu-id="c58aa-102">Maximum Length</span></span>

<span data-ttu-id="c58aa-103">Skonfigurowanie maksymalnej długości zapewnia wskazówkę do magazynu danych o odpowiednim typie danych, który ma być używany dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="c58aa-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="c58aa-104">Maksymalna długość dotyczy tylko typów danych tablicowych, takich jak `string` i `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c58aa-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="c58aa-105">Entity Framework nie sprawdza żadnej weryfikacji maksymalnej długości przed przekazaniem danych do dostawcy.</span><span class="sxs-lookup"><span data-stu-id="c58aa-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="c58aa-106">Jest to dostawca lub magazyn danych do zweryfikowania, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="c58aa-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="c58aa-107">Na przykład podczas określania wartości docelowej SQL Server przekroczenie maksymalnej długości spowoduje wyjątek, ponieważ typ danych kolumny źródłowej nie zezwala na przechowywanie nadmiarowych danych.</span><span class="sxs-lookup"><span data-stu-id="c58aa-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="c58aa-108">Konwencje</span><span class="sxs-lookup"><span data-stu-id="c58aa-108">Conventions</span></span>

<span data-ttu-id="c58aa-109">Zgodnie z Konwencją, pozostało do dostawcy bazy danych, aby wybrać odpowiedni typ danych dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="c58aa-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="c58aa-110">Dla właściwości, które mają długość, dostawca bazy danych zwykle wybiera typ danych, który pozwala na najdłuższą długość danych.</span><span class="sxs-lookup"><span data-stu-id="c58aa-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="c58aa-111">Na przykład Microsoft SQL Server będzie używać `nvarchar(max)` dla `string` właściwości (lub `nvarchar(450)` Jeśli kolumna jest używana jako klucz).</span><span class="sxs-lookup"><span data-stu-id="c58aa-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c58aa-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="c58aa-112">Data Annotations</span></span>

<span data-ttu-id="c58aa-113">Możesz użyć adnotacji danych, aby skonfigurować maksymalną długość właściwości.</span><span class="sxs-lookup"><span data-stu-id="c58aa-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="c58aa-114">W tym przykładzie element docelowy SQL Server spowoduje `nvarchar(500)` to użycie typu danych.</span><span class="sxs-lookup"><span data-stu-id="c58aa-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="c58aa-115">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="c58aa-115">Fluent API</span></span>

<span data-ttu-id="c58aa-116">Możesz użyć interfejsu API Fluent, aby skonfigurować maksymalną długość właściwości.</span><span class="sxs-lookup"><span data-stu-id="c58aa-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="c58aa-117">W tym przykładzie element docelowy SQL Server spowoduje `nvarchar(500)` to użycie typu danych.</span><span class="sxs-lookup"><span data-stu-id="c58aa-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
