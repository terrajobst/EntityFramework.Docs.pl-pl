---
title: Właściwości wymagane/opcjonalne-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149150"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="e02eb-102">Właściwości wymagane i opcjonalne</span><span class="sxs-lookup"><span data-stu-id="e02eb-102">Required and Optional Properties</span></span>

<span data-ttu-id="e02eb-103">Właściwość jest uważana za opcjonalną, jeśli jest poprawna `null`, aby mogła ją zawierać.</span><span class="sxs-lookup"><span data-stu-id="e02eb-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="e02eb-104">Jeśli `null` nie jest prawidłową wartością do przypisania do właściwości, zostanie ona uznana za właściwość wymaganą.</span><span class="sxs-lookup"><span data-stu-id="e02eb-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="e02eb-105">Konwencje</span><span class="sxs-lookup"><span data-stu-id="e02eb-105">Conventions</span></span>

<span data-ttu-id="e02eb-106">Zgodnie z Konwencją właściwość, której typ .NET może zawierać wartość null, zostanie skonfigurowana jako`string`opcjonalna `byte[]`(, `int?`, itp.).</span><span class="sxs-lookup"><span data-stu-id="e02eb-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="e02eb-107">Właściwości, których typ CLR nie może zawierać wartości null, zostaną skonfigurowane`int`jako `decimal`wymagane `bool`(,, itp.).</span><span class="sxs-lookup"><span data-stu-id="e02eb-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="e02eb-108">Nie można skonfigurować właściwości, której typ .NET nie może zawierać wartości null.</span><span class="sxs-lookup"><span data-stu-id="e02eb-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="e02eb-109">Właściwość będzie zawsze uznawana za wymaganą przez Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e02eb-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e02eb-110">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="e02eb-110">Data Annotations</span></span>

<span data-ttu-id="e02eb-111">Możesz użyć adnotacji danych, aby wskazać, że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="e02eb-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="e02eb-112">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="e02eb-112">Fluent API</span></span>

<span data-ttu-id="e02eb-113">Możesz użyć interfejsu API Fluent, aby wskazać, że właściwość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="e02eb-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

