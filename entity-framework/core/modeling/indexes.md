---
title: Indeksy — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655695"
---
# <a name="indexes"></a><span data-ttu-id="932b3-102">Zwiększa</span><span class="sxs-lookup"><span data-stu-id="932b3-102">Indexes</span></span>

<span data-ttu-id="932b3-103">Indeksy są typowymi koncepcjami w wielu magazynach danych.</span><span class="sxs-lookup"><span data-stu-id="932b3-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="932b3-104">Chociaż ich implementacja w magazynie danych może się różnić, są one używane do wykonywania wyszukiwania na podstawie kolumny (lub zestawu kolumn) bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="932b3-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="932b3-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="932b3-105">Conventions</span></span>

<span data-ttu-id="932b3-106">Zgodnie z Konwencją indeks jest tworzony we wszystkich właściwościach (lub zestawach właściwości), które są używane jako klucz obcy.</span><span class="sxs-lookup"><span data-stu-id="932b3-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="932b3-107">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="932b3-107">Data Annotations</span></span>

<span data-ttu-id="932b3-108">Nie można tworzyć indeksów przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="932b3-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="932b3-109">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="932b3-109">Fluent API</span></span>

<span data-ttu-id="932b3-110">Możesz użyć interfejsu API Fluent, aby określić indeks dla pojedynczej właściwości.</span><span class="sxs-lookup"><span data-stu-id="932b3-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="932b3-111">Domyślnie indeksy nie są unikatowe.</span><span class="sxs-lookup"><span data-stu-id="932b3-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="932b3-112">Można również określić, że indeks powinien być unikatowy, co oznacza, że żadne dwie jednostki nie mogą mieć tych samych wartości dla danej właściwości.</span><span class="sxs-lookup"><span data-stu-id="932b3-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="932b3-113">Można również określić indeks w więcej niż jednej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="932b3-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="932b3-114">Istnieje tylko jeden indeks dla każdego odrębnego zestawu właściwości.</span><span class="sxs-lookup"><span data-stu-id="932b3-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="932b3-115">Jeśli używasz interfejsu API Fluent do skonfigurowania indeksu dla zestawu właściwości, który ma już zdefiniowany indeks, w ramach Konwencji lub poprzedniej konfiguracji, zmienisz definicję tego indeksu.</span><span class="sxs-lookup"><span data-stu-id="932b3-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="932b3-116">Jest to przydatne, jeśli chcesz skonfigurować indeks, który został utworzony przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="932b3-116">This is useful if you want to further configure an index that was created by convention.</span></span>
