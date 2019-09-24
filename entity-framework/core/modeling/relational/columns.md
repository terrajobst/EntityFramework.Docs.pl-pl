---
title: Mapowanie kolumn — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197215"
---
# <a name="column-mapping"></a><span data-ttu-id="a0992-102">Mapowanie kolumn</span><span class="sxs-lookup"><span data-stu-id="a0992-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="a0992-103">Konfiguracja w tej sekcji jest ogólnie stosowana do relacyjnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="a0992-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a0992-104">Przedstawione tutaj metody rozszerzania staną się dostępne po zainstalowaniu dostawcy relacyjnej bazy danych (ze względu na współużytkowany pakiet *Microsoft. EntityFrameworkCore. relacyjny* ).</span><span class="sxs-lookup"><span data-stu-id="a0992-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a0992-105">Mapowanie kolumn identyfikuje, które dane kolumny mają być wysyłane i zapisywane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="a0992-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="a0992-106">Konwencje</span><span class="sxs-lookup"><span data-stu-id="a0992-106">Conventions</span></span>

<span data-ttu-id="a0992-107">Zgodnie z Konwencją Każda właściwość zostanie skonfigurowana do mapowania do kolumny o tej samej nazwie co właściwość.</span><span class="sxs-lookup"><span data-stu-id="a0992-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a0992-108">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="a0992-108">Data Annotations</span></span>

<span data-ttu-id="a0992-109">Możesz użyć adnotacji danych, aby skonfigurować kolumnę, do której zostanie zamapowana właściwość.</span><span class="sxs-lookup"><span data-stu-id="a0992-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="a0992-110">Interfejs API Fluent</span><span class="sxs-lookup"><span data-stu-id="a0992-110">Fluent API</span></span>

<span data-ttu-id="a0992-111">Korzystając z interfejsu API Fluent, można skonfigurować kolumnę, do której jest zamapowana właściwość.</span><span class="sxs-lookup"><span data-stu-id="a0992-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
