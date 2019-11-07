---
title: Kolumny obliczane — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655920"
---
# <a name="computed-columns"></a><span data-ttu-id="a7ecd-102">Kolumny obliczane</span><span class="sxs-lookup"><span data-stu-id="a7ecd-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="a7ecd-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="a7ecd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a7ecd-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="a7ecd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a7ecd-105">Kolumna obliczana to kolumna, której wartość jest obliczana w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a7ecd-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="a7ecd-106">Kolumna obliczana może użyć innych kolumn w tabeli do obliczenia jej wartości.</span><span class="sxs-lookup"><span data-stu-id="a7ecd-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="a7ecd-107">Konwencje</span><span class="sxs-lookup"><span data-stu-id="a7ecd-107">Conventions</span></span>

<span data-ttu-id="a7ecd-108">Według Konwencji kolumny obliczane nie są tworzone w modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ecd-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a7ecd-109">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="a7ecd-109">Data Annotations</span></span>

<span data-ttu-id="a7ecd-110">Kolumn obliczanych nie można skonfigurować za pomocą adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="a7ecd-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a7ecd-111">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="a7ecd-111">Fluent API</span></span>

<span data-ttu-id="a7ecd-112">Możesz użyć interfejsu API Fluent, aby określić, że właściwość powinna być mapowana na kolumnę obliczaną.</span><span class="sxs-lookup"><span data-stu-id="a7ecd-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
