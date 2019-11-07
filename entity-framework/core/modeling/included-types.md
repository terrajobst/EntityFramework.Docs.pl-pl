---
title: Uwzględnienie & z wyjątkiem typów — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655736"
---
# <a name="including--excluding-types"></a><span data-ttu-id="f865e-102">Uwzględnianie i wykluczanie typów</span><span class="sxs-lookup"><span data-stu-id="f865e-102">Including & Excluding Types</span></span>

<span data-ttu-id="f865e-103">Uwzględnienie typu w modelu oznacza, że Dr ma metadane dotyczące tego typu i spróbuje odczytywać i zapisywać wystąpienia z/do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f865e-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f865e-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f865e-104">Conventions</span></span>

<span data-ttu-id="f865e-105">Zgodnie z Konwencją, typy, które są uwidocznione we właściwościach `DbSet` w Twoim kontekście, są uwzględniane w modelu.</span><span class="sxs-lookup"><span data-stu-id="f865e-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="f865e-106">Ponadto są uwzględnione również typy, które są wymienione w metodzie `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="f865e-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="f865e-107">Na koniec wszystkie typy, które są znalezione przez rekursywnie Eksplorowanie właściwości nawigacji odnalezionych typów, są również uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="f865e-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="f865e-108">**Na przykład w poniższym kodzie wykryto wszystkie trzy typy:**</span><span class="sxs-lookup"><span data-stu-id="f865e-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="f865e-109">`Blog`, ponieważ jest on uwidoczniony we właściwości `DbSet` w kontekście</span><span class="sxs-lookup"><span data-stu-id="f865e-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="f865e-110">`Post`, ponieważ został on odnaleziony za pośrednictwem właściwości nawigacji `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="f865e-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="f865e-111">`AuditEntry`, ponieważ jest on wymieniony w `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="f865e-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="f865e-112">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f865e-112">Data Annotations</span></span>

<span data-ttu-id="f865e-113">Możesz użyć adnotacji danych, aby wykluczyć typ z modelu.</span><span class="sxs-lookup"><span data-stu-id="f865e-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="f865e-114">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="f865e-114">Fluent API</span></span>

<span data-ttu-id="f865e-115">Możesz użyć interfejsu API Fluent do wykluczenia typu z modelu.</span><span class="sxs-lookup"><span data-stu-id="f865e-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
