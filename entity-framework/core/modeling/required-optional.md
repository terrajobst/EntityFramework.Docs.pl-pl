---
title: Właściwości wymagane/opcjonalne — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929813"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="eeeff-102">Wymagane i opcjonalne właściwości</span><span class="sxs-lookup"><span data-stu-id="eeeff-102">Required and Optional Properties</span></span>

<span data-ttu-id="eeeff-103">Właściwości są traktowane jako opcjonalne, jeśli jest on prawidłowy, aby mogła zawierać `null`.</span><span class="sxs-lookup"><span data-stu-id="eeeff-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="eeeff-104">Jeśli `null` nie jest prawidłową wartością ma być przypisane do właściwości, a następnie jest on uznawany za jest właściwością wymaganą.</span><span class="sxs-lookup"><span data-stu-id="eeeff-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="eeeff-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="eeeff-105">Conventions</span></span>

<span data-ttu-id="eeeff-106">Zgodnie z Konwencją, właściwość, której typ CLR może zawierać wartości null zostanie skonfigurowany jako opcjonalny (`string`, `int?`, `byte[]`itp.).</span><span class="sxs-lookup"><span data-stu-id="eeeff-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="eeeff-107">Właściwości, których typ CLR nie może zawierać wartości null zostanie skonfigurowany zgodnie z wymogami (`int`, `decimal`, `bool`itp.).</span><span class="sxs-lookup"><span data-stu-id="eeeff-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="eeeff-108">Właściwość, której typ CLR nie może zawierać wartości null nie można skonfigurować jako opcjonalną.</span><span class="sxs-lookup"><span data-stu-id="eeeff-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="eeeff-109">Właściwość będzie zawsze być brana pod uwagę wymagane przez program Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="eeeff-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="eeeff-110">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="eeeff-110">Data Annotations</span></span>

<span data-ttu-id="eeeff-111">Korzystanie z adnotacji danych, aby wskazać, że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="eeeff-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="eeeff-112">Interfejs Fluent API</span><span class="sxs-lookup"><span data-stu-id="eeeff-112">Fluent API</span></span>

<span data-ttu-id="eeeff-113">Aby wskazać, że właściwość jest wymagana, można użyć interfejsu API Fluent.</span><span class="sxs-lookup"><span data-stu-id="eeeff-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

