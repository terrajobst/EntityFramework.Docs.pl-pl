---
title: Uwzględnianie i wykluczanie właściwości — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929827"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="f79c8-102">Uwzględnianie i wykluczanie właściwości</span><span class="sxs-lookup"><span data-stu-id="f79c8-102">Including & Excluding Properties</span></span>

<span data-ttu-id="f79c8-103">W tym właściwości w modelu oznacza, że EF ma metadane dotyczące tej właściwości i podejmie próbę odczytu i zapisu wartości z i do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f79c8-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f79c8-104">Konwencje</span><span class="sxs-lookup"><span data-stu-id="f79c8-104">Conventions</span></span>

<span data-ttu-id="f79c8-105">Zgodnie z Konwencją właściwości publicznej metody pobierającej i ustawiającej zostaną uwzględnione w modelu.</span><span class="sxs-lookup"><span data-stu-id="f79c8-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f79c8-106">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f79c8-106">Data Annotations</span></span>

<span data-ttu-id="f79c8-107">Korzystanie z adnotacji danych, które mają zostać wykluczone z właściwością z modelu.</span><span class="sxs-lookup"><span data-stu-id="f79c8-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="f79c8-108">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="f79c8-108">Fluent API</span></span>

<span data-ttu-id="f79c8-109">Interfejs Fluent API umożliwia wykluczać właściwość z modelu.</span><span class="sxs-lookup"><span data-stu-id="f79c8-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
