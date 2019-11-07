---
title: Sekwencje — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656114"
---
# <a name="sequences"></a><span data-ttu-id="539c7-102">Sekwencje</span><span class="sxs-lookup"><span data-stu-id="539c7-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="539c7-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="539c7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="539c7-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="539c7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="539c7-105">Sekwencja generuje sekwencyjne wartości liczbowe w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="539c7-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="539c7-106">Sekwencje nie są skojarzone z określoną tabelą.</span><span class="sxs-lookup"><span data-stu-id="539c7-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="539c7-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="539c7-107">Conventions</span></span>

<span data-ttu-id="539c7-108">Zgodnie z Konwencją sekwencje nie są wprowadzane w modelu.</span><span class="sxs-lookup"><span data-stu-id="539c7-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="539c7-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="539c7-109">Data Annotations</span></span>

<span data-ttu-id="539c7-110">Nie można skonfigurować sekwencji przy użyciu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="539c7-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="539c7-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="539c7-111">Fluent API</span></span>

<span data-ttu-id="539c7-112">Za pomocą interfejsu API Fluent można utworzyć sekwencję w modelu.</span><span class="sxs-lookup"><span data-stu-id="539c7-112">You can use the Fluent API to create a sequence in the model.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

<span data-ttu-id="539c7-113">Możesz również skonfigurować dodatkowy aspekt sekwencji, taki jak jego schemat, wartość początkowa i przyrost.</span><span class="sxs-lookup"><span data-stu-id="539c7-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

<span data-ttu-id="539c7-114">Po wprowadzeniu sekwencji można użyć jej do generowania wartości właściwości w modelu.</span><span class="sxs-lookup"><span data-stu-id="539c7-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="539c7-115">Na przykład można użyć [wartości domyślnych](default-values.md) , aby wstawić następną wartość z sekwencji.</span><span class="sxs-lookup"><span data-stu-id="539c7-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
